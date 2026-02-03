
# Telegram Business Scraper Bot (n8n + Apify)

A simple Telegram bot that lets you scrape local businesses directly from chat.

Screenshots can be found in documents/. The screenshot of the telegram bot result is blurred, respecting the privacy.

Send a message like:

cafes in helsinki 5

And the bot will:
- scrape businesses using Apify (Google Maps scraper)
- return results in Telegram
- automatically save them to Google Sheets
- track outreach with a "Contacted" column

Built with:
- n8n (workflow automation)
- Telegram Bot API
- Apify scraping actors
- Google Sheets
- ngrok (public webhook tunnel)

---

## 🚀 Features

- Chat-based business search
- Natural language input (no commands required)
- Dynamic result limit
- Saves all leads to Google Sheets
- Adds "Contacted" column for manual tracking
- Runs locally with public webhook via ngrok
- Fully self-hosted (no paid hosting required)

---

## 🧠 How it works

Each Telegram message triggers a new workflow execution:

User message  
→ Telegram Trigger (n8n webhook)  
→ Parse query (business + city + limit)  
→ Apify scrape  
→ Format results  
→ Save rows to Google Sheets  
→ Reply in Telegram  

The bot is **stateless**:
- 1 message = 1 execution
- no loops or waiting
- fast and reliable

---

## 🛠 Tech Stack

- Node.js
- n8n
- Telegram Bot API
- Apify
- Google Sheets API
- ngrok

---

## 📦 Installation

### 1. Install n8n

```bash
npm install -g n8n
````

Run:

```bash
n8n
```

Default:

```
http://localhost:5678
```

---

### 2. Install ngrok

Linux:

```bash
sudo apt install ngrok
```

Start tunnel:

```bash
ngrok http 5678
```

Example output:

```
https://xxxxx.ngrok-free.dev -> http://localhost:5678
```

Copy the HTTPS URL.

---

### 3. Configure n8n Webhook URL

Set environment variable:

```bash
export WEBHOOK_URL=https://xxxxx.ngrok-free.dev
```

Restart n8n.

This allows Telegram to reach your local server.

---

## 🤖 Telegram Setup

### Create bot

Open BotFather:

```
/newbot
```

Copy:

* Bot token

Add token to Telegram credentials inside n8n.

---

## 📊 Google Sheets Setup

1. Create spreadsheet
2. Add header row:

Title | City | Address | Phone | Website | PlaceId | Contacted

3. Create Google OAuth credentials in Google Cloud
4. Connect Google Sheets node in n8n
5. Use "Append" operation

---

## 🕷 Apify Setup

1. Create account on Apify
2. Get API token
3. Use HTTP Request node:

POST:

```
https://api.apify.com/v2/acts/<actor>/runs
```

Query param:

```
token=YOUR_API_TOKEN
```

Body:

```json
{
  "searchStringsArray": ["query"],
  "locationQuery": "city",
  "maxCrawledPlacesPerSearch": limit
}
```

---

## 💬 Usage

Send message:

```
cafes in helsinki 5
```

Format:

```
<business> in <city> <number>
```

Examples:

```
plumbers in tampere 10
restaurants in turku 20
construction in helsinki 15
```

---

## 📋 Google Sheets Output

Each run appends:

| Title | City | Address | Phone | Website | PlaceId | Contacted |
| ----- | ---- | ------- | ----- | ------- | ------- | --------- |

Set `Contacted = X` manually after outreach.

---

## 🧩 Workflow Structure (n8n)

```
Telegram Trigger
 → Set context
 → Parse text
 → Apify HTTP Request
 → Format results
 ↘ Telegram reply
 ↘ Google Sheets append
```

Parallel branches:

* one for Telegram
* one for Sheets

---

## ⚠️ Important (ngrok note)

This project uses **ngrok** to expose the local n8n server.

Because ngrok free URLs change every restart:

After restarting ngrok you MUST:

1. copy new HTTPS URL
2. update WEBHOOK_URL
3. restart n8n

Otherwise Telegram webhooks will fail.

---

## 🔮 Possible Improvements

* paging (/next)
* deduplicate leads
* Supabase database
* email extraction
* deployment to VPS
* inline Telegram buttons

---

## 📄 License

MIT


