---
name: telegram-bot
description: Build Telegram bots for notifications, alerts, and automation. Use when adding Telegram integration to a project, sending automated messages, creating command-based bots, or building notification systems. You already use this in setup_telegram.py in the AI_Business_Builder project.
argument-hint: [bot-purpose]
---

# Telegram Bot Builder

You are an expert in building Telegram bots with Python using python-telegram-bot. Help create bots for notifications, alerts, interactive commands, and automation pipelines.

## Setup

```bash
pip install python-telegram-bot python-dotenv
```

Create your bot: Message [@BotFather](https://t.me/BotFather) on Telegram → `/newbot` → get your `BOT_TOKEN`

Get your Chat ID: Start your bot, then visit:
`https://api.telegram.org/bot<BOT_TOKEN>/getUpdates`

## Simple Notification Sender (for pipelines)

This is the most common pattern — just send a message when something happens:

```python
# utils/telegram_notify.py
import requests
import os

BOT_TOKEN = os.getenv("TELEGRAM_BOT_TOKEN")
CHAT_ID = os.getenv("TELEGRAM_CHAT_ID")

def notify(message: str, parse_mode: str = "HTML") -> bool:
    """Send a Telegram notification. Returns True on success."""
    if not BOT_TOKEN or not CHAT_ID:
        return False
    
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendMessage"
    payload = {
        "chat_id": CHAT_ID,
        "text": message,
        "parse_mode": parse_mode
    }
    
    try:
        res = requests.post(url, json=payload, timeout=10)
        res.raise_for_status()
        return True
    except Exception as e:
        print(f"Telegram notification failed: {e}")
        return False

def notify_success(title: str, details: str = ""):
    notify(f"✅ <b>{title}</b>\n{details}")

def notify_error(title: str, error: str = ""):
    notify(f"❌ <b>{title}</b>\n<code>{error}</code>")

def notify_progress(step: str, current: int, total: int):
    bar = "█" * (current * 10 // total) + "░" * (10 - current * 10 // total)
    notify(f"⏳ {step}\n[{bar}] {current}/{total}")
```

## Usage in Your Pipelines

```python
# In main.py — wrap your pipeline with notifications
from utils.telegram_notify import notify_success, notify_error

def run():
    notify("🚀 <b>Pipeline gestartet</b>")
    try:
        results = process()
        notify_success("Pipeline fertig!", f"{len(results)} Videos verarbeitet")
    except Exception as e:
        notify_error("Pipeline fehlgeschlagen", str(e))
        raise
```

## Full Command Bot (Interactive)

```python
# bot.py — Full bot with commands
from telegram import Update
from telegram.ext import Application, CommandHandler, MessageHandler, filters, ContextTypes
import os

TOKEN = os.getenv("TELEGRAM_BOT_TOKEN")

async def start(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text(
        "👋 Hallo! Ich bin dein Automation Bot.\n\n"
        "/status — Pipeline Status\n"
        "/run — Pipeline starten\n"
        "/help — Hilfe"
    )

async def status(update: Update, context: ContextTypes.DEFAULT_TYPE):
    # Check pipeline status
    await update.message.reply_text("✅ Alles läuft normal.")

async def run_pipeline(update: Update, context: ContextTypes.DEFAULT_TYPE):
    await update.message.reply_text("🚀 Pipeline wird gestartet...")
    # Trigger your pipeline here
    await update.message.reply_text("✅ Pipeline gestartet!")

async def handle_message(update: Update, context: ContextTypes.DEFAULT_TYPE):
    text = update.message.text
    await update.message.reply_text(f"Du hast geschrieben: {text}")

def main():
    app = Application.builder().token(TOKEN).build()
    
    app.add_handler(CommandHandler("start", start))
    app.add_handler(CommandHandler("status", status))
    app.add_handler(CommandHandler("run", run_pipeline))
    app.add_handler(MessageHandler(filters.TEXT & ~filters.COMMAND, handle_message))
    
    print("Bot läuft...")
    app.run_polling()

if __name__ == "__main__":
    main()
```

## Send Files (Photos, Videos, Documents)

```python
import requests

def send_photo(image_path: str, caption: str = ""):
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendPhoto"
    with open(image_path, "rb") as f:
        requests.post(url, data={"chat_id": CHAT_ID, "caption": caption}, files={"photo": f})

def send_video(video_path: str, caption: str = ""):
    url = f"https://api.telegram.org/bot{BOT_TOKEN}/sendVideo"
    with open(video_path, "rb") as f:
        requests.post(url, data={"chat_id": CHAT_ID, "caption": caption}, files={"video": f})
```

## .env Template

```
TELEGRAM_BOT_TOKEN=1234567890:ABCdef...
TELEGRAM_CHAT_ID=-1001234567890  # negative = group chat
```

## Instructions

1. Ask what the bot should do (notifications only, or interactive commands)
2. Ask which project this is for — add it as `utils/telegram_notify.py` for simple use
3. For notification bots: use the simple HTTP approach (no library needed)
4. For command bots: use python-telegram-bot library
5. Always load credentials from `.env`, never hardcode tokens
6. Add `notify_success` and `notify_error` to the pipeline's `main.py` automatically
