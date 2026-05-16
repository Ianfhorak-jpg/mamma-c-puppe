# Ian's Project Memory

Auto-generated from all projects in C.C.Projekts_Ian. Refreshed every session.

**Current project:** 18_Maman_website_pouper
**Generated:** 2026-05-16 16:34

---

## Skills Available in This Project

| Skill | Purpose |
|-------|---------|
| skill-builder | Use when creating new skills, optimizing existing skills, or auditing  |
| canvas-design | Create beautiful visual art in .png and .pdf documents using design ph |
| context-engineering | >- |
| frontend-design | Create distinctive, production-grade frontend interfaces with high des |
| marketing-content-strategy | When the user wants to plan a content strategy, decide what content to |
| marketing-copywriting | When the user wants to write, rewrite, or improve marketing copy for a |
| marketing-cro | When the user wants to optimize, improve, or increase conversions on a |
| marketing-seo | When the user wants to audit, review, or diagnose SEO issues on their  |
| reference | Migrate a component from one framework to another |
| remotion | Best practices for Remotion - Video creation in React |
| stop-slop | Remove AI writing patterns from prose. Use when drafting, editing, or  |
| ui-ux-pro-max | "UI/UX design intelligence for web and mobile. Includes 50+ styles, 16 |

---

## Previous Projects

### 01_dirndlnamfeld_website

**Files:** CNAME,SKILL.md,dns-records-for-sarah.md,github-pages-custom-domain-guide.md,images,index.html,reference.md,screenshots

---

### 02_Papa_Website

**CLAUDE.md (first 40 lines):**
```
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Single-page static website for **Dirndln am Feld**, a bio market gardening farm in Kirchberg am Wagram, Austria. The entire site is one file: `index.html`. There is no build step, no package manager, no framework.

**Live site:** https://ianfhorak-jpg.github.io/dirndlnamfeld/
**Deployment:** GitHub Pages — push `index.html` and any new image files to the repo root on `main`.

## File layout

All files live flat in the repo root — no subdirectories:
- `index.html` — the entire site (HTML + CSS + JS in one file)
- `*.jpg` — all images, referenced by filename only (e.g. `src="4amfeld.jpg"`)

Image filenames with spaces or special characters (e.g. `1erstes Bild .jpg`, `dirndln&mehr.jpg`) are used directly as `src` values — browsers handle the encoding automatically.

## External dependencies (CDN only)

Loaded at the bottom of `index.html`, no local copies:
- **GSAP 3.12.5** + **ScrollTrigger** — scroll-driven animations
- **Lenis 1.0.42** — smooth scroll (fault-tolerant: wrapped in try/catch)

## CSS architecture

All styles are in a `<style>` block inside `<head>`. Design tokens live in `:root` and should be used for all color and spacing changes:

| Token | Value | Purpose |
|---|---|---|
| `--sage` | `#C5962B` | Brand gold — accent color, eyebrows, highlights |
| `--cream` | `#EAE4D6` | Primary background |
| `--mint` | `#D5ECE8` | Secondary/alternate section background |
| `--black` | `#0D0D0D` | Nav, footer, primary buttons |
| `--mono` | Andale Mono / Courier | Site-wide font |

## Animation system

Scroll reveal is class-based. Add one of these classes to any element to animate it on scroll:
```

**Files:** CLAUDE.md,DRINDL-AM-FELD-TAG-011797-768x1024.jpg,DSCF0044-768x1024.jpg,DSCF0158-768x576.jpg,DSCF0203-768x1024.jpg,DSCF6373-768x1024.jpg,DSCF7089-768x576.jpg,DSCF7820-768x1024.jpg,DSC_6224-768x1152.jpg,MA-48-Zauber0105-fertig-neu-01-hoch-1-768x1024.jpg,MINISEX-20250386-768x1024.jpg,app,components,lib,next-env.d.ts

---

### 03_AI_Agent_1

**CLAUDE.md (first 40 lines):**
```
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm test                    # run all tests headless (Chromium only)
HEADED=1 npm test           # run tests with a visible browser
PWDEBUG=1 npm test          # step-through debugger
npm run test:ui             # open Playwright's interactive UI
npm run test:report         # open the last HTML report

# Run a single test file
npx playwright test tests/example.spec.ts

# Run a single test by name
npx playwright test -g "page title matches"

# Screenshot script
npm run screenshot -- https://example.com screenshots/out.png
# or: npx tsx scripts/screenshot.ts <url> <output.png>

# Automation script
npm run automate
```

## Onboarding Form

`public/index.html` is a self-contained multi-step onboarding form (12 questions, one per screen). No build step required — all CSS and JS are embedded.

```bash
npm run serve               # serve the form at http://localhost:3000
open public/index.html      # open directly in browser (file:// — also works)

# Run only the form tests (uses file:// — no server needed)
npx playwright test tests/onboarding.spec.ts
```

The form persists answers in `localStorage` under the key `onboarding_v1` so users can resume after a refresh. The thank-you screen clears that key.
```

**Files:** CLAUDE.md,onboarding.html,package-lock.json,package.json,playwright-report,playwright.config.ts,public,screenshots,scripts,test-results,tests

---

### 04_reels_cutter

**Files:** __pycache__,assets,config.py,main.py,pipeline,processed_output,requirements.txt

---

### 05_Youtube_shorts

**Project Status:**
```
# AI Viral Video Pipeline — Project Status

**Last updated:** 2026-05-04  
**Status:** ✅ Working — dry run completed successfully

---

## What Was Built

A fully automated Python pipeline that generates brainrot Reddit videos and posts them daily to YouTube Shorts, TikTok, and Instagram Reels. No n8n, no monthly subscriptions — everything runs from your Mac.

**How it works (in plain English):**
1. GPT-4o-mini writes a Reddit-style story script
2. OpenAI TTS reads it aloud in a dramatic voice
3. Downloads a free gaming background video from Pexels
4. Combines video + voice into a 9:16 vertical MP4
5. (Optionally) burns animated subtitles onto the video
6. Posts automatically to YouTube, TikTok, Instagram

---

## Dry Run Result (2026-05-04)

```
Run ID:     c2f30eec
Title:      My Roommate Stole My Skincare... and This Happened
Script:     86 words (r/AITA topic)
Audio:      29.7 seconds
Video:      15MB → output/final/c2f30eec_final.mp4
Total cost: $0.0075 per video
```

**Files:** PROJECT_STATUS.md,Skills,__pycache__,assembly,assets,claude-code-brainrot-toolkit,config.py,content,data,logs,main.py,media,output,pipeline,publishers

---

### 06_AI_Business_Builder

**Files:** 01_Business_Pläne,02_Umsetzungspläne,AI_Ideen_Auswahl.md,generate_pdfs.py

---

### 07_AI_Video_Clipper

**CLAUDE.md (first 40 lines):**
```
# AI Video Clipper

Automatische Pipeline die aus einem langen Video (Talking Head Interview, Podcast, Webinar) 
so viele hochwertige, postbare Kurzclips wie möglich extrahiert.

## Was dieses Projekt ist

- **Input:** 1-stündiges Video (lokale Datei oder YouTube-Link)
- **Output:** N Clips in 9:16 (TikTok/Reels) + 16:9 (YouTube) mit eingebrannten Untertiteln
- **Kosten:** ~0.10-0.20 € pro Video

## Pipeline

```
Video → Whisper Transkription → Claude API Clip-Auswahl → FFmpeg Schnitt → Untertitel → Output
```

## Relevante Dateien

- `Businessplan_AI_Video_Clipping.md` — Businesskontext, Pricing, Kunden
- `Plan_AI_Video_Clipping.md` — Ursprünglicher Umsetzungsplan (Woche 1-3)
- `.claude/plans/PIPELINE_PLAN.md` — Detaillierter Implementierungsplan (aktuell)

## Tech Stack

- `openai-whisper` — Transkription mit word_timestamps
- `anthropic` (Claude API) — Clip-Auswahl & Virality-Bewertung
- `ffmpeg` — Video schneiden + 9:16 crop
- `moviepy` + `Pillow` — Karaoke-Untertitel einbrennen
- `yt-dlp` — YouTube-Links herunterladen

## Setup (noch nicht gemacht)

```bash
brew install ffmpeg
pip install openai-whisper anthropic ffmpeg-python yt-dlp Pillow moviepy==1.0.3 numpy imageio-ffmpeg
```

## Verwendung (nach dem Bauen)
```

**Files:** Businessplan_AI_Video_Clipping.md,CLAUDE.md,Plan_AI_Video_Clipping.md,__pycache__,config.py,logs,main.py,output,pipeline,requirements.txt,tools,utils

---

### 08_AI_Voice_Agent

**Files:** Businessplan_AI_Voice_Agent.pdf,README.md,requirements.txt,src,templates

---

### 09_Protokoll_Niedel

**Files:** 1000067412.jpg,1000067414.jpg,1000067415.jpg,Niedel_1.xlsx,Protokoll_Messbedingungen_IanFayeHorak.docx,Schachner_Protokol_IanFH.docx

---

### 10_Neuer_Ordner

**Files:** Businessplan_KI_Print_Automation.md,PROJECT_REFERENCE.md,Plan_Ian_Druckerei_KI_DETAIL.docx,Plan_KI_Print_Automation.md,Proposal_Druckerei_Pilot.docx

---

### 11_Higsfileld

**Files:** ad-creatives,assets,generated-videos,skills-lock.json,weekly-content

---

### 12_Luis_Website

**Files:** Veloura_Advertorials.zip,Veloura_Package,assets,build_package.py,listicle-preview.jpeg,migraine-advertorial.html,migraine-fixed.jpeg,migraine-full.jpeg,migraine-listicle.html,migraine-preview.jpeg,neck-fixed.jpeg,neck-pain-advertorial.html,neck-pain-listicle.html,neck-pain-preview.jpeg,reference-full.png

---

### 13_Video_Editing

**Files:** __pycache__,assets,config.py,main.py,pipeline,processed_output,raw_input,reels_lotta_hering Kopie.mp4,requirements.txt

---

### 14_Investment_Agent

**CLAUDE.md (first 40 lines):**
```
# Investment Agent

AI-powered stock signal bot. Monitors US stocks using 4 signal layers
(technical, news sentiment, macro, multi-timeframe) and sends Telegram
buy/sell alerts in EUR.

## Run

```bash
python main.py          # start live scheduler (every 15 min, market hours)
python run_once.py --ticker AAPL              # full test, sends Telegram
python run_once.py --ticker NVDA --no-telegram  # dry run, prints to terminal
```

## Setup

1. Copy `.env.example` to `.env` and fill in all API keys
2. Edit `config/settings.yaml` to set your investment goal
3. Edit `config/watchlist.yaml` to add/remove stocks
4. Run `python run_once.py --ticker AAPL --no-telegram` to verify

## API Keys needed

- `ANTHROPIC_API_KEY` — claude.ai
- `TELEGRAM_BOT_TOKEN` + `TELEGRAM_CHAT_ID` — @BotFather on Telegram
- `FINNHUB_API_KEY` — finnhub.io (free)
- `ALPHA_VANTAGE_API_KEY` — alphavantage.co (free)

## Architecture

```
yfinance → market_data.py → indicators.py  ┐
finnhub  → news_fetcher.py                 ├→ claude_agent.py → Signal → telegram_bot.py
yfinance → macro_context.py               ┘
```

## Phase Roadmap

- Phase 1 (current): Notification-only, paper trading
- Phase 2: Paper portfolio P&L tracker, daily digest
```

**Files:** CLAUDE.md,__pycache__,agent.db,charts,config,logs,main.py,requirements.txt,run_once.py,setup_telegram.py,src

---

### 15_Productivity

**Files:** 

---

### 16_Ai_news_Agent

**Files:** 

---

### 17_Tennis_Optimma

**Files:** -,OPTIMA - Film Head Facts.pdf,OPTIMA Fragenkatalog Tennisverbandstrainer.pdf,OPTIMA KONZEPT - KI GESTEUERTE TENNIS APP.pdf,OPTIMA Marktforschung Eltern.pdf,PLAN.md,SETUP.md,tennis_zone

---

### 18_Maman_website_pouper

**CLAUDE.md (first 40 lines):**
```
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Website for **Maman** — purpose and tech stack TBD. Project is in its initial setup phase; no source files exist yet.

Update this section once the stack is chosen (e.g. static HTML, Next.js, Astro) and the site's goal is clear.

## Context within Ian's projects

Similar reference projects in `C.C.Projekts_Ian`:

| Project | Stack | Notes |
|---|---|---|
| `01_dirndlnamfeld_website` | Static HTML (single file) | Inline CSS/JS, GitHub Pages |
| `02_Papa_Website` | Next.js | GitHub Pages deployment |
| `12_Luis_Website` | Static HTML advertorial | High-converting landing page style |

## Skills available in this project

These Claude Code skills are installed and relevant for a website build:

| Skill | When to use |
|---|---|
| `/frontend-design` | Building components, pages, layouts |
| `/ui-ux-pro-max` | Design systems, color palettes, font pairings |
| `/marketing-copywriting` | Homepage, landing page, hero copy |
| `/marketing-cro` | Conversion optimization |
| `/marketing-seo` | SEO audit and on-page optimization |
| `/stop-slop` | Clean up AI-written copy before publishing |
| `/canvas-design` | Visual assets (.png / .pdf) |

## Notes to add as the project develops

- **Deployment target** — GitHub Pages / Vercel / Netlify / other
- **Domain / live URL**
- **Design tokens** — brand colors, fonts, spacing
- **Content structure** — sections, pages
```

**Files:** CLAUDE.md,buecher.html,evelyne-award.png,evelyne-home.png,evelyne-portrait.png,evelyne-redesign-v1.png,evelyne-redesign.html,filme.html,index.html,kontakt.html,mamma-c-puppe-full.png,mamma-v2.png,mamma-v3.png,netlify-access.png,netlify-check.png

---

### 19_Statup_Building_App

**Files:** 

---

