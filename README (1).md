# RouteIQ — AI Sales Insights from Trip Data

A prototype AI-powered business intelligence dashboard for transport/logistics owners. Upload trip data and get instant AI-generated insights — no manual Excel analysis needed.

![RouteIQ Screenshot](https://img.shields.io/badge/Powered%20by-Claude%20Sonnet%204-00e5b4?style=for-the-badge)

## Features

- **Upload trip data** — CSV, Excel, or JSON
- **Auto-computed KPIs** — Revenue, margin, delay rate, trip count
- **Top revenue routes** — with margin % visualisation
- **Delayed routes** — ranked by average delay in minutes
- **High-value customers** — ranked by total spend
- **Low-margin routes** — flagged in red/amber for action
- **AI narrative summary** — plain-English business summary powered by Claude
- **Conversational Q&A** — ask any question about your data (RAG-powered chat)
- **Sample data** — 20 realistic Indian logistics trips to demo instantly

## Tech Stack

| Layer | Technology |
|---|---|
| LLM | Claude Sonnet 4 (`claude-sonnet-4-20250514`) |
| Prompt Engineering | Structured data context injected into system prompt |
| RAG Pattern | Computed stats fed as retrieval context per query |
| Chatbot | Multi-turn conversation with full history |
| Data Analysis | Client-side JS pipeline: raw rows → stats → AI narrative |
| Summarisation | Owner-friendly output, no spreadsheet knowledge needed |
| Frontend | Vanilla HTML/CSS/JS — zero dependencies, zero build step |

## CSV Format

Your CSV should have these columns (case-sensitive):

```
trip_id, route, customer, revenue, cost, delay_mins, date, vehicle
```

Example:
```csv
trip_id,route,customer,revenue,cost,delay_mins,date,vehicle
T001,Hyderabad → Mumbai,Reliance Logistics,42000,31000,0,2024-01-05,TN09AB1234
T002,Chennai → Bangalore,TCS Freight,18500,16200,45,2024-01-07,AP01CD5678
```

## Setup

1. Clone the repo
2. Open `index.html` in a browser
3. The app calls the Anthropic API via `https://api.anthropic.com/v1/messages` — API key is handled by the claude.ai artifact proxy when running inside Claude. To run standalone, add your API key to the fetch headers:

```js
headers: {
  "Content-Type": "application/json",
  "x-api-key": "YOUR_ANTHROPIC_API_KEY",
  "anthropic-version": "2023-06-01"
}
```

## WhatsApp Integration

The AI summary text is plain string output — ready to pipe into the WhatsApp Business API:

```js
// After getting analysisContext from Claude:
await fetch("https://api.whatsapp.com/v1/messages", {
  method: "POST",
  headers: { Authorization: "Bearer YOUR_TOKEN" },
  body: JSON.stringify({
    to: "91XXXXXXXXXX",
    type: "text",
    text: { body: analysisContext }
  })
});
```

## Project Structure

```
ai-sales-insights/
├── index.html      # Full app (single file — HTML + CSS + JS)
└── README.md
```

## Built With

- [Claude API](https://docs.anthropic.com) by Anthropic
- [Tabler Icons](https://tabler.io/icons)
- [Syne](https://fonts.google.com/specimen/Syne) + [DM Sans](https://fonts.google.com/specimen/DM+Sans) + [DM Mono](https://fonts.google.com/specimen/DM+Mono) via Google Fonts

---

> Prototype built as part of an AI tools portfolio — demonstrating LLMs, prompt engineering, RAG, chatbot frameworks, data analysis with LLMs, and messaging API integration.
