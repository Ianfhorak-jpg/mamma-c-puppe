---
name: ai-news-agent
description: Build AI-powered news aggregation agents that scrape, filter, summarize, and deliver news digests. Use when building news scrapers, LLM-powered content filters, automated digests via Telegram/email, or extending the AI_news_Agent project.
argument-hint: [topic] [delivery-method]
---

# AI News Agent Builder

You are an expert in building automated news aggregation systems. Help create agents that scrape news, use LLMs to filter and summarize, and deliver clean digests via Telegram, email, or web.

## Architecture Overview

```
Sources          Filter        Summarize       Deliver
─────────        ──────        ─────────       ───────
RSS Feeds   →   LLM Score  →  GPT Summary  →  Telegram
NewsAPI     →   Keyword     →  Bullet List  →  Email
Web Scrape  →   Relevance   →  Full Digest  →  Dashboard
Reddit      →   Dedup       →              →  Webhook
```

## News Sources Setup

```python
# pipeline/stage_01_collect.py
import feedparser
import requests
from datetime import datetime, timedelta
from dataclasses import dataclass

NEWS_API_KEY = os.getenv("NEWS_API_KEY")  # newsapi.org — free tier: 100/day

@dataclass
class NewsItem:
    title: str
    summary: str
    url: str
    source: str
    published: datetime
    score: float = 0.0

def fetch_rss(feed_url: str) -> list[NewsItem]:
    """Fetch articles from an RSS feed."""
    feed = feedparser.parse(feed_url)
    items = []
    cutoff = datetime.now() - timedelta(hours=24)
    
    for entry in feed.entries:
        published = datetime(*entry.published_parsed[:6]) if hasattr(entry, "published_parsed") else datetime.now()
        if published < cutoff:
            continue
        items.append(NewsItem(
            title=entry.title,
            summary=getattr(entry, "summary", "")[:500],
            url=entry.link,
            source=feed.feed.title,
            published=published
        ))
    return items

def fetch_newsapi(topic: str, language: str = "de") -> list[NewsItem]:
    """Fetch from NewsAPI."""
    res = requests.get(
        "https://newsapi.org/v2/everything",
        params={
            "q": topic, "language": language,
            "from": (datetime.now() - timedelta(days=1)).isoformat(),
            "sortBy": "relevancy", "apiKey": NEWS_API_KEY,
            "pageSize": 20
        }
    )
    articles = res.json().get("articles", [])
    return [
        NewsItem(
            title=a["title"] or "",
            summary=a["description"] or "",
            url=a["url"],
            source=a["source"]["name"],
            published=datetime.fromisoformat(a["publishedAt"].replace("Z", "+00:00"))
        )
        for a in articles if a["title"] and "[Removed]" not in a["title"]
    ]

# Popular RSS feeds by topic:
RSS_FEEDS = {
    "tech": [
        "https://techcrunch.com/feed/",
        "https://feeds.arstechnica.com/arstechnica/index",
    ],
    "ai": [
        "https://www.artificialintelligence-news.com/feed/",
        "https://rss.beehiiv.com/feeds/z9WGFK5jY3.xml",  # The Rundown AI
    ],
    "business": [
        "https://feeds.bloomberg.com/technology/news.rss",
        "https://www.ft.com/news-feed?format=rss",
    ],
    "austria": [
        "https://www.derstandard.at/rss",
        "https://kurier.at/feed",
    ]
}
```

## LLM Filter & Scorer

```python
# pipeline/stage_02_filter.py
from openai import OpenAI
import json

client = OpenAI()

def score_articles(items: list[NewsItem], topic: str, persona: str) -> list[NewsItem]:
    """Score articles 0-10 for relevance. Filter out scores < 6."""
    if not items:
        return []
    
    articles_json = json.dumps([
        {"id": i, "title": item.title, "summary": item.summary}
        for i, item in enumerate(items)
    ])
    
    response = client.chat.completions.create(
        model="gpt-4o-mini",  # Fast and cheap for filtering
        response_format={"type": "json_object"},
        messages=[
            {"role": "system", "content": f"You score news articles for a {persona} interested in {topic}. Return JSON: {{\"scores\": [{{\"id\": 0, \"score\": 8.5}}]}}"},
            {"role": "user", "content": articles_json}
        ]
    )
    
    scores = json.loads(response.choices[0].message.content)["scores"]
    score_map = {s["id"]: s["score"] for s in scores}
    
    for i, item in enumerate(items):
        item.score = score_map.get(i, 0)
    
    return [item for item in items if item.score >= 6.0]

def deduplicate(items: list[NewsItem]) -> list[NewsItem]:
    """Remove near-duplicate stories."""
    seen_titles = set()
    unique = []
    for item in sorted(items, key=lambda x: x.score, reverse=True):
        # Simple word-overlap dedup
        words = set(item.title.lower().split())
        if not any(len(words & set(t.split())) / max(len(words), 1) > 0.6 for t in seen_titles):
            seen_titles.add(item.title.lower())
            unique.append(item)
    return unique
```

## Digest Generator

```python
# pipeline/stage_03_summarize.py
from openai import OpenAI

client = OpenAI()

def generate_digest(items: list[NewsItem], topic: str, language: str = "de") -> str:
    """Generate a clean news digest."""
    articles_text = "\n\n".join([
        f"TITLE: {item.title}\nSOURCE: {item.source}\nSUMMARY: {item.summary}\nURL: {item.url}"
        for item in items[:10]
    ])
    
    response = client.chat.completions.create(
        model="gpt-4o-mini",
        messages=[
            {"role": "system", "content": f"Du bist ein Nachrichtenredakteur. Erstelle einen kurzen, prägnanten Digest auf {language} über: {topic}"},
            {"role": "user", "content": f"Fasse diese Artikel zusammen:\n\n{articles_text}"}
        ]
    )
    return response.choices[0].message.content
```

## Scheduler (Run Daily)

```python
# scheduler/daily_digest.py
import schedule
import time
from pipeline.stage_01_collect import fetch_rss, fetch_newsapi, RSS_FEEDS
from pipeline.stage_02_filter import score_articles, deduplicate
from pipeline.stage_03_summarize import generate_digest
from utils.telegram_notify import notify

def run_digest():
    topic = "Artificial Intelligence"
    items = []
    for feed in RSS_FEEDS["ai"]:
        items.extend(fetch_rss(feed))
    items.extend(fetch_newsapi(topic))
    
    items = score_articles(items, topic, "tech entrepreneur")
    items = deduplicate(items)
    
    digest = generate_digest(items, topic)
    notify(f"📰 <b>KI News Digest</b>\n\n{digest}")

schedule.every().day.at("08:00").do(run_digest)

while True:
    schedule.run_pending()
    time.sleep(60)
```

## Instructions

1. Ask what topics to track and who the audience is
2. Ask how to deliver the digest (Telegram, email, both)
3. Ask how often to run (daily at 8am, multiple times, etc.)
4. Default to `gpt-4o-mini` for scoring/filtering (cheap), `gpt-4o` for final digest (quality)
5. Always add deduplication — same news from multiple sources is common
6. Match the existing `AI_news_Agent/src/` structure when extending that project
