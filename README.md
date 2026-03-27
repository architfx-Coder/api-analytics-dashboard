#  API-Integrated Analytics Dashboard

> **A live, multi-source analytics dashboard that auto-refreshes data from 3+ public APIs — zero manual data entry.**

[![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=flat-square&logo=streamlit&logoColor=white)](https://streamlit.io)
[![Plotly](https://img.shields.io/badge/Plotly-3F4F75?style=flat-square&logo=plotly&logoColor=white)](https://plotly.com)
[![REST APIs](https://img.shields.io/badge/REST_APIs-009688?style=flat-square)]()
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg?style=flat-square)](LICENSE)

🔗 **[Live Demo →](https://your-streamlit-app.streamlit.app)**

---

##  Problem Statement

Business analysts waste hours every week copying data from multiple sources into spreadsheets — news sites, financial data portals, economic databases — before any analysis even begins. By the time a report is ready, the data is already stale.

**The core question:** *Can we build a single dashboard that pulls live data from multiple APIs, processes it automatically, and presents decision-ready insights — refreshed on demand, with no manual work?*

---

##  Objective

1. Connect to **3+ real-world public APIs** and unify their data in a single interface
2. Apply **real-time analytics** logic (aggregations, trend detection, alerting)
3. Build an interactive dashboard requiring **zero manual data entry**
4. Demonstrate that automation removes the "data collection" bottleneck entirely
5. Design the UX so a non-technical user can extract insights without any help

---

##  Data Sources (APIs Integrated)

| API | Data Type | Refresh Rate | Use in Dashboard |
|-----|-----------|-------------|-----------------|
| **Alpha Vantage / Yahoo Finance** | Stock prices, FX rates | Real-time | Price tracker, % change |
| **World Bank Open Data API** | Macro indicators (GDP, inflation, unemployment) | Monthly | Economic context panel |
| **NewsAPI / GNews** | Financial news headlines | Hourly | Sentiment + news feed |
| **Open Exchange Rates** | Currency exchange rates | Real-time | FX rate table |
| **REST Countries API** | Country metadata | Static | Geo context labels |

>  *All APIs use free tiers — no paid subscription required to run this project.*

---

##  Dashboard Panels

### Panel 1 — Market Overview
- Live price cards: selected tickers with % change (daily / weekly)
- Sparkline charts for 30-day price history
- Volume trend bars

### Panel 2 — Macro Intelligence
- GDP growth rates: top 10 economies (bar chart, sortable)
- Inflation vs. unemployment scatter plot (Phillips Curve visualisation)
- Country-level macro table with conditional formatting

### Panel 3 — FX Rate Monitor
- Live currency rate table vs. USD base
- Heatmap of % change across major pairs (last 7 days)
- Trend line: user-selected pair over custom date range

### Panel 4 — News & Sentiment Feed
- Latest 20 financial headlines (auto-refreshed)
- **Automated sentiment scoring** per headline (positive / neutral / negative) using VADER
- Sentiment trend chart: proportion of positive vs. negative news over time
- Keyword frequency bar chart from headlines (topic tracking)

---

##  Methodology

### Step 1 — API Integration Layer
- Built a modular `api_client.py` with dedicated fetch functions per source
- Implemented caching (`@st.cache_data` with TTL) to avoid redundant API calls
- Error handling for rate limits, timeouts, and malformed responses
- `.env` file for secure API key management

### Step 2 — Data Processing Pipeline
- Normalised all JSON responses into Pandas DataFrames
- Aligned timestamps across sources (UTC standardisation)
- Computed derived metrics: 7-day rolling average, % change, z-score for anomaly flagging

### Step 3 — Sentiment Analysis
- Applied VADER (Valence Aware Dictionary Sentiment Reasoner) to news headlines
- Classified each headline as Positive / Neutral / Negative
- Aggregated sentiment scores by day and ticker mention

### Step 4 — Dashboard Build (Streamlit)
- Modular panel structure: each panel is an independent component
- User controls: date range picker, ticker selector, country filter, refresh button
- Colour-coded conditional formatting (green = positive, red = negative)
- Mobile-responsive layout using Streamlit columns

### Step 5 — Deployment
- Deployed to Streamlit Community Cloud (free hosting)
- Environment variables managed via Streamlit Secrets
- Refresh interval: on-demand (button) + auto-refresh every 15 minutes

---

##  Tools & Technologies

| Category | Tools |
|----------|-------|
| Language | Python 3.10 |
| Dashboard | Streamlit |
| Visualisation | Plotly Express, Plotly Graph Objects |
| API Handling | Requests, httpx |
| Data | Pandas, NumPy |
| NLP / Sentiment | NLTK (VADER) |
| Env Management | python-dotenv |
| Deployment | Streamlit Community Cloud |
| Version Control | Git / GitHub |

---

##  Key Insights

1. **API reliability varies significantly.** World Bank API is highly stable but slow (~2–3 seconds). Alpha Vantage free tier has rate limits (5 calls/min) — required smart caching to maintain UX.

2. **Sentiment diverges from price movement ~30% of the time.** Negative headlines often coincided with positive price action — a useful contrarian signal worth exploring further.

3. **GDP data lags by 1–2 quarters** in the World Bank API — crucial for users to understand so macro context is not mistaken for current conditions. Added a clear data freshness label.

4. **Single-source dashboards miss the story.** The most valuable insights emerge at the intersection of news sentiment + price movement + macro backdrop — something impossible to see when data lives in three separate tools.

---

##  Visualisations Included

-  **Live Price Sparklines** — 30-day mini-charts per ticker
-  **FX Change Heatmap** — Major currency pairs, 7-day % change
-  **Sentiment Timeline** — Daily sentiment ratio chart
-  **GDP Bubble Chart** — Economy size vs. growth rate
-  **Keyword Word Cloud** — Most discussed topics in news headlines
-  **Macro Comparison Table** — With conditional cell colouring

---

##  Business Impact

| Stakeholder | Value Delivered |
|-------------|----------------|
| **Analyst / Trader** | Replaces 2–3 hours/day of manual data collection |
| **Portfolio Manager** | Single-view macro + market context for morning briefings |
| **Business Development** | Country-level economic signals for market entry decisions |
| **Content Creator / Writer** | Live data and sentiment trends for market commentary |

> 🏢 This type of integrated dashboard is used by Bloomberg Terminal users, FX desks, and BI teams at major banks — replicated here using entirely free tools.

---

##  Repository Structure

```
api-analytics-dashboard/
│
├── api/
│   ├── market_data.py        # Alpha Vantage / Yahoo Finance client
│   ├── macro_data.py         # World Bank API client
│   ├── news_data.py          # NewsAPI client
│   └── fx_data.py            # Open Exchange Rates client
│
├── processing/
│   ├── clean.py              # Data normalisation
│   ├── sentiment.py          # VADER sentiment scoring
│   └── metrics.py            # Derived metrics (% change, rolling avg)
│
├── dashboard/
│   ├── panels/
│   │   ├── market_overview.py
│   │   ├── macro_panel.py
│   │   ├── fx_panel.py
│   │   └── news_panel.py
│   └── app.py                # Main Streamlit entry point
│
├── .env.example              # API key template (never commit real keys)
├── requirements.txt
└── README.md
```

---

##  How to Run

```bash
git clone https://github.com/yourusername/api-analytics-dashboard.git
cd api-analytics-dashboard

pip install -r requirements.txt

# Set up your API keys
cp .env.example .env
# Edit .env and add your API keys

# Launch dashboard
streamlit run dashboard/app.py
```

---

##  Future Improvements

- [ ] **Alert System** — Email/Slack notifications when a macro indicator or price crosses a threshold
- [ ] **Database Layer** — Store fetched data in SQLite for historical backtesting
- [ ] **Multi-Currency Portfolio Tracker** — Add portfolio input and PnL calculation
- [ ] **AI Summary Button** — Use GPT-4 to auto-generate a daily market summary from live data
- [ ] **User Authentication** — Personalised watchlists and saved preferences via Streamlit Auth

---

## 📜 License

This project is licensed under the [MIT License](LICENSE).

---

<div align="center">
<sub>Built by [Your Name] · Chennai, India · 2024</sub>
</div>
