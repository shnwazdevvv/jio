# 🎵 JioSaavn Bot — Render Deployment Guide

Deploy this Telegram bot to [Render.com](https://render.com) in minutes.

---

## 📋 Prerequisites

| Requirement | Where to get it |
|-------------|----------------|
| Telegram API ID & Hash | [my.telegram.org/apps](https://my.telegram.org/apps) |
| Bot Token | [@BotFather](https://t.me/BotFather) on Telegram |
| MongoDB URI *(optional)* | [MongoDB Atlas](https://www.mongodb.com/cloud/atlas) — free tier |
| Render account | [render.com](https://render.com) — free tier works |

---

## 🚀 Deploy to Render

### Option A — One-click via `render.yaml` (recommended)

1. **Fork / push** this repo to your GitHub account.
2. Go to [dashboard.render.com](https://dashboard.render.com) → **New → Blueprint**.
3. Connect your GitHub repo. Render will detect `render.yaml` automatically.
4. Fill in the environment variables (see below) and click **Apply**.

### Option B — Manual setup

1. Go to **New → Web Service** on Render.
2. Connect your GitHub repo.
3. Set:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `python -m jiosaavn`
4. Add environment variables (see below).
5. Click **Create Web Service**.

---

## 🔑 Environment Variables

Set these in **Render Dashboard → Your Service → Environment**:

| Variable | Required | Description |
|----------|----------|-------------|
| `API_ID` | ✅ | Telegram API ID |
| `API_HASH` | ✅ | Telegram API Hash |
| `BOT_TOKEN` | ✅ | Bot token from @BotFather |
| `DATABASE_URL` | ⚠️ optional | MongoDB URI for user settings |
| `RENDER_URL` | ⚠️ recommended | Your Render app URL — keeps the bot alive on free tier |
| `PING_INTERVAL` | ❌ | Ping interval in seconds (default: 600) |

---

## 🔄 Keeping the Bot Alive (Free Tier)

Render's free tier **spins down after 15 minutes of inactivity**. To prevent this:

1. After your first deploy, copy your service URL (e.g. `https://jiosaavn-bot.onrender.com`).
2. Add it as the `RENDER_URL` environment variable.
3. The bot pings itself every 10 minutes to stay awake.

> **Tip:** For a always-on bot, upgrade to Render's **Starter plan** (~$7/month).

---

## 📁 Project Structure

```
jiosaavn/
├── __main__.py          # Entry point
├── bot.py               # Pyrogram Client
├── app_webpage.py       # Health-check web server (port 10000)
├── config/
│   └── settings.py      # Env variable config
├── database/
│   └── database.py      # MongoDB integration
└── plugins/             # Bot command handlers
    ├── commands.py
    ├── search_handler.py
    ├── download_handler.py
    ├── songs_handler.py
    ├── playlist_or_album_handler.py
    ├── artist_handler.py
    ├── settings_handler.py
    └── text.py

render.yaml              # ← Render deploy config (new)
Procfile                 # ← Process definition
.env.example             # ← Environment variable template (new)
requirements.txt
```

---

## 🐛 Troubleshooting

| Problem | Fix |
|---------|-----|
| Bot not responding | Check `BOT_TOKEN` is correct in env vars |
| Service keeps restarting | Add `RENDER_URL` to enable self-ping |
| Database errors | Verify `DATABASE_URL` is a valid MongoDB URI |
| Port errors | Leave `PORT` unset — Render injects it automatically |
