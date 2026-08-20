# 🎯 Jumia Price Sniper Bot

Telegram bot that fetches live product data from Jumia Nigeria, filters out accessories and price outliers, verifies trusted vendors, and sends automated price-drop alerts.

## ✨ Features

- ⚡ **Fast & Lightweight Engine** — Uses `curl_cffi` with TLS fingerprint impersonation (`chrome120`) to fetch raw Jumia catalog data directly, bypassing Cloudflare without heavy browser overhead (no Playwright needed).
- 🧠 **AI Query Correction** — Powered by Gemini AI to correct typos, normalize brand names, and optimize search queries.
- 🛡️ **Smart Filtering** — Automatically screens out accessories (cases, screen protectors, pouches, glass) and filters out price outliers.
- ✅ **Trust Verification** — Detects "Official Store" and "Jumia Express" badges to prioritize trusted vendors.
- 📊 **Market Analysis** — Computes listing counts, price ranges, and highlights the best trusted deal.
- 🔔 **Price Tracking & Alerts** — Track products and get notified automatically when prices drop below your threshold (checks every 6 hours).
- 💾 **Search Cache** — Disk-cached query results to save API quota and speed up repeated searches.

## 🛠️ Requirements

- Python 3.11+
- Telegram Bot Token from [@BotFather](https://t.me/BotFather)
- Gemini API Key

## ⚙️ Setup

1. **Install Dependencies**

   ```bash
   pip install -r requirements.txt
   ```

2. **Configure Environment Variables**

   Create or update your `.env` file in the root directory:

   ```env
   TELEGRAM_TOKEN=your_bot_token_here
   GEMINI_API_KEY=your_gemini_key_here
   CHAT_ID=your_chat_id_here
   DB_NAME=price_sniper_bot
   DB_USER=postgres
   DB_PASSWORD=your_password
   DB_HOST=localhost
   DB_PORT=5432
   ```

## 🎮 Usage

Start the bot:

```bash
python -m src.main
```

### Bot Commands

| Command | Description |
|---------|-------------|
| `/start` | Welcome message & quick guide |
| `/help`  | Command reference |
| `/search <query>` | Search Jumia catalog (e.g. `/search iPhone 15`) |

After searching, click **Track Price Drops** under a product to receive automatic notifications whenever the price drops.

## 📁 Project Layout

```
src/
├── main.py               # Entry point (runs polling loop)
├── config.py             # Pydantic settings loaded from .env
├── bot/
│   ├── app.py            # Application builder & background job setup
│   └── handlers.py       # Telegram command & callback handlers
├── scraper/
│   ├── jumia.py          # curl_cffi HTTP request client & catalog parser
│   └── cache.py          # Search query cache
├── services/
│   ├── search.py         # Search service orchestrator
│   ├── query_cleaner.py  # Gemini AI query optimizer
│   └── price_checker.py  # Background price monitor & alerter
└── database/
    ├── interface.py      # Database interface
    ├── json_db.py        # JSON database storage
    └── postgres.py       # PostgreSQL database adapter
data/
├── database.json         # Tracked products store
└── search_cache.json     # Cached search results
tests/                    # Unit tests
```

## 🚨 Price Alerts

The bot regularly monitors tracked products. If a price drop is detected, you will receive an alert like:

> 🔔 **Price Drop Alert!**  
> **Redmi 13C**  
> 📉 **Was:** ₦190,000  
> 🏷️ **Now:** ₦150,000  
> 🔗 [View on Jumia](https://www.jumia.com.ng/...)

The tracked alert price automatically updates to the lower amount so you only get notified on new drops.
