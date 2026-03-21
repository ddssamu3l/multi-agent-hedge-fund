# Data Pipeline Design: How Agents Get Information

> How real-time, periodic, and reference information flows into each agent's context.

---

## Architecture Overview

```
                    ┌─────────────────────────────────┐
                    │       INGESTION LAYER            │
                    │   (cron jobs, runs before agents  │
                    │    wake each morning)             │
                    │                                   │
  REAL-TIME FEEDS   │   DEPTH SOURCES    KNOWLEDGE BASE │
  ─────────────────►│◄───────────────   ──────────────►│
  Market data       │   YouTube          Books (parsed) │
  News RSS          │   Podcasts         Our own docs   │
  EDGAR filings     │   Substacks        Historical DB  │
  FRED macro data   │   Earnings calls                  │
  Sentiment feeds   │                                   │
                    └──────────┬────────────────────────┘
                               │
                               │ context processor (cheap LLM)
                               │ summarizes, tags, routes
                               │
                    ┌──────────▼────────────────────────┐
                    │       NOTIFICATION ROUTER          │
                    │                                    │
                    │  Routes summaries to agents based   │
                    │  on: domain relevance, subscriptions,│
                    │  keyword triggers, priority level    │
                    └──────────┬────────────────────────┘
                               │
              ┌────────────────┼────────────────────┐
              ▼                ▼                     ▼
     ┌──────────────┐ ┌──────────────┐    ┌──────────────┐
     │ MACRO AGENT  │ │  SEMI AGENT  │    │ CHINA AGENT  │
     │              │ │              │    │              │
     │ Morning feed:│ │ Morning feed:│    │ Morning feed:│
     │ - FRED data  │ │ - TSMC rev   │    │ - PBoC moves │
     │ - Fed minutes│ │ - SEMI data  │    │ - Caixin PMI │
     │ - Macro YT   │ │ - Chip YT    │    │ - China YT   │
     │ - Bond mkt   │ │ - ASML orders│    │ - Trade data │
     └──────────────┘ └──────────────┘    └──────────────┘

     Full source data stored in archive.
     Agents recall full text on demand via recall() tool.
```

---

## Source Categories

### Category 1: Market Data (Continuous / Daily)

| Source | API | Cost | Update Frequency | What It Provides |
|--------|-----|------|-----------------|------------------|
| Yahoo Finance | `yfinance` Python lib | Free | Real-time (15min delay) | Price, volume, fundamentals for any ticker |
| FRED | `fredapi` Python lib | Free (API key required, instant) | Varies by series | 800K+ economic data series: GDP, CPI, unemployment, yields, money supply, everything |
| Alpha Vantage | REST API | Free tier: 25 calls/day | Daily/intraday | Price data, technical indicators, forex, crypto |
| CoinGecko | REST API | Free tier: 30 calls/min | Real-time | Crypto prices, market cap, volume, DeFi data |
| CFTC COT | Weekly CSV download | Free | Weekly (Friday) | Futures positioning by trader type (commercial, non-commercial, leveraged) |

**Implementation:** Cron job pulls market data daily at market close. Store in local DB (SQLite or PostgreSQL). Agents query via tools.

```python
# Example: FRED macro data pull
from fredapi import Fred
fred = Fred(api_key='...')

# Key series every macro agent needs
series = {
    'DGS10': '10-Year Treasury Yield',
    'DGS2': '2-Year Treasury Yield',
    'CPIAUCSL': 'CPI (All Urban Consumers)',
    'UNRATE': 'Unemployment Rate',
    'FEDFUNDS': 'Federal Funds Rate',
    'M2SL': 'M2 Money Supply',
    'WALCL': 'Fed Balance Sheet Total Assets',
    'DTWEXBGS': 'Trade-Weighted Dollar Index',
    'BAMLH0A0HYM2': 'High Yield Credit Spread',
}

for series_id, name in series.items():
    data = fred.get_series(series_id, observation_start='2024-01-01')
```

---

### Category 2: News & Filings (Continuous / Daily)

| Source | Method | Cost | What It Provides |
|--------|--------|------|------------------|
| NewsAPI.org | REST API | Free: 100 req/day | Headlines + snippets from 80K+ sources, filterable by keyword/source/date |
| SEC EDGAR | REST API (no key needed) | Free | 10-K, 10-Q, 8-K filings, insider transactions (Form 4), 13F institutional holdings |
| FRED Press Releases | RSS | Free | Fed statements, FOMC minutes, Beige Book |
| Financial news RSS | `feedparser` Python lib | Free | Bloomberg, Reuters, FT, Nikkei, Caixin, SCMP, etc. |

**RSS Feeds for Financial News (curated):**

```python
NEWS_FEEDS = {
    # English - Major outlets
    'reuters_markets': 'https://www.reutersagency.com/feed/?taxonomy=best-sectors&post_type=best',
    'ft_markets': 'https://www.ft.com/markets?format=rss',
    'bloomberg_markets': 'https://feeds.bloomberg.com/markets/news.rss',

    # Asia-focused
    'nikkei_asia': 'https://asia.nikkei.com/rss',
    'scmp_economy': 'https://www.scmp.com/rss/5/feed',

    # China-focused (English)
    'caixin_global': 'https://www.caixinglobal.com/rss.html',

    # Macro / Central Banks
    'fed_press': 'https://www.federalreserve.gov/feeds/press_all.xml',
    'ecb_press': 'https://www.ecb.europa.eu/rss/press.html',
    'boj_announcements': 'https://www.boj.or.jp/en/rss/whatsnew.xml',

    # Semiconductor-specific
    'semiwiki': 'https://semiwiki.com/feed/',
    'anandtech': 'https://www.anandtech.com/rss/',
    'tom_hardware': 'https://www.tomshardware.com/feeds/all',
}
```

**SEC EDGAR Pipeline:**

```python
# Fetch recent filings for tracked companies
import requests

# EDGAR full-text search (free, no key)
# Example: find recent 10-K filings for NVDA
url = "https://efts.sec.gov/LATEST/search-index"
params = {
    "q": "NVIDIA",
    "dateRange": "custom",
    "startdt": "2025-01-01",
    "enddt": "2026-03-21",
    "forms": "10-K,10-Q,8-K"
}
# Must set User-Agent header (SEC requires it)
headers = {"User-Agent": "HedgeFundResearch contact@example.com"}
```

**Insider Transaction Monitoring:**

```python
# OpenInsider (free, scrape) or SEC EDGAR Form 4
# Key signals: cluster selling by C-suite across a sector
# Maps directly to Crack Signal #2 (Insider Behavior Divergence)
```

---

### Category 3: YouTube Channels (Daily Morning Pull)

**Discovery: RSS (primary, free, no API key)**

```python
import feedparser

def get_recent_videos(channel_id, hours=24):
    """Fetch videos published in last N hours from a channel."""
    feed_url = f"https://www.youtube.com/feeds/videos.xml?channel_id={channel_id}"
    feed = feedparser.parse(feed_url)

    cutoff = datetime.now(timezone.utc) - timedelta(hours=hours)
    recent = []

    for entry in feed.entries:
        published = datetime.fromisoformat(entry.published)
        if published > cutoff:
            recent.append({
                'video_id': entry.yt_videoid,
                'title': entry.title,
                'published': entry.published,
                'description': entry.summary,
                'url': entry.link,
            })

    return recent
```

**Transcript Extraction:**

```python
from youtube_transcript_api import YouTubeTranscriptApi

def get_transcript(video_id):
    """Get transcript text from a YouTube video."""
    try:
        transcript = YouTubeTranscriptApi.get_transcript(video_id)
        full_text = ' '.join([entry['text'] for entry in transcript])
        return full_text
    except Exception:
        # Fallback: yt-dlp subtitle extraction
        return get_transcript_ytdlp(video_id)

def get_transcript_ytdlp(video_id):
    """Fallback: extract subtitles via yt-dlp."""
    import subprocess, json
    result = subprocess.run([
        'yt-dlp',
        '--write-auto-subs',
        '--skip-download',
        '--sub-format', 'json3',
        '--output', '/tmp/%(id)s',
        f'https://youtube.com/watch?v={video_id}'
    ], capture_output=True, text=True)
    # Parse the json3 subtitle file
    # ... (post-processing to plain text)
```

**Summarization via Context Processor:**

```python
def summarize_video(title, transcript, model="cheap-model"):
    """Summarize a YouTube transcript for agent consumption."""
    prompt = f"""Summarize this video transcript for a financial analyst.

Title: {title}

Extract:
- KEY CLAIMS (specific, falsifiable statements)
- DATA POINTS (numbers, dates, metrics mentioned)
- PREDICTIONS (any forward-looking statements with timeframes)
- TICKERS MENTIONED (any company names or stock symbols)
- SENTIMENT (bullish/bearish/neutral on what)

Keep under 300 tokens. Be specific, not vague.

Transcript:
{transcript[:8000]}  # Cap input to control costs
"""
    return call_llm(prompt, model=model)
```

**Agent Subscription Model:**

Each agent's config (or identity.md) contains a subscription list:

```yaml
# In agent config / identity.md
subscriptions:
  youtube:
    - channel_id: UCxxxxxxx  # Macro Voices
      priority: high
    - channel_id: UCyyyyyyy  # China Economics
      priority: medium
  rss:
    - url: https://www.ft.com/markets?format=rss
      priority: high
    - url: https://asia.nikkei.com/rss
      priority: medium
  edgar:
    tickers: [NVDA, TSMC, ASML, AAPL, MSFT]
    forms: [10-K, 10-Q, 8-K, Form4]
  fred:
    series: [DGS10, DGS2, CPIAUCSL, FEDFUNDS]
```

**Routing logic:** The ingestion layer checks each agent's subscriptions and compiles a personalized morning feed. High-priority items always included; medium-priority items included if relevant keywords match the agent's domain.

---

### Category 4: Podcasts & Substacks (Daily / Weekly)

**Podcasts:**
Many financial podcasts publish transcripts or detailed show notes.
For those that don't, the same transcript approach works:
1. RSS feed → detect new episodes
2. Download audio → Whisper (local or API) → transcript
3. Summarize via context processor

Cost consideration: Whisper API is ~$0.006/minute. A 1-hour podcast = $0.36. Running locally with whisper.cpp is free but slower.

**Substacks / Blogs:**
- Most have RSS feeds (`substack-name.substack.com/feed`)
- `feedparser` → get new posts → extract full text
- Summarize via context processor
- Many top financial analysts publish on Substack (free tier posts)

```python
SUBSTACK_FEEDS = {
    # Examples of high-quality financial substacks
    'matt_levine': 'https://www.bloomberg.com/opinion/authors/ARbTQlRLRjE/matthew-s-levine',  # Bloomberg
    # Add curated substacks here
}
```

---

### Category 5: Earnings Call Transcripts (Quarterly)

Critical for Earnings/Fundamentals agent and Semiconductor agent.

| Source | Method | Cost |
|--------|--------|------|
| SEC EDGAR | Free, raw filing format | Free |
| Seeking Alpha | Web scrape (ToS concerns) | Free |
| Financial Modeling Prep API | REST API | Free tier: 250 calls/day |
| Earnings call audio → Whisper | Transcribe ourselves | Compute cost |

**Recommended approach:**
1. Track earnings calendar for watched tickers
2. Pull transcripts from Financial Modeling Prep API (free tier sufficient for ~50 companies/quarter)
3. Context processor extracts: revenue/EPS vs consensus, guidance changes, key quotes from management, capex plans, supply chain commentary
4. Summary goes to relevant agent; full transcript in archive

---

### Category 6: Knowledge Base (Loaded Once)

**Books:**
- Parse to plain text (epub → text, PDF → text via `pymupdf` or `pdfplumber`)
- Chunk into ~400-token blocks with 80-token overlap
- Store in searchable format (keyword index, future: vector DB)
- Agents recall relevant passages via `recall("Pettis savings glut")`

**Our own reference docs:**
- `docs/knowledge/world-mechanics.md`
- `docs/knowledge/reasoning-examples.md`
- `docs/knowledge/exit-signals.md`
- Already designed for keyword pre-injection

**Historical data:**
- Past agent predictions + actual outcomes (for track record)
- Historical earnings, macro data (for base rate calculations)
- Stored in DB, queryable via agent tools

---

## The Morning Feed: What An Agent Sees When It Wakes

```
══════════════════════════════════════════════════
MORNING BRIEFING — Semiconductor Supply Chain Agent
2026-03-22 06:00 UTC
══════════════════════════════════════════════════

MARKET DATA (auto-populated):
  NVDA: $892.40 (+1.2%)  TSM: $178.20 (-0.3%)
  ASML: $945.00 (+0.8%)  MU: $112.50 (+2.1%)
  SOX Index: 5,240 (+0.9%)
  DRAM Spot: $2.45 (unchanged)  NAND Spot: $0.08 (-1%)

NEW FILINGS:
  [8-K] NVIDIA — Announced new GPU architecture event Apr 15
  [Form4] TSMC — CFO sold 12,000 shares at $178.50

SUBSCRIBED YOUTUBE (2 new videos):
  ► "TSMC CoWoS Capacity Crisis Deepening" — TechChannel
    KEY: CoWoS capacity booked through Q2 2027. New Arizona
    fab delayed 3 months. HBM allocation favoring NVIDIA over
    AMD. Bearish AMD near-term, bullish packaging companies.
    [recall("full transcript, TSMC CoWoS") for detail]

  ► "Japan Chip Equipment Orders Surge" — SemiAnalysis
    KEY: Tokyo Electron orders +45% QoQ. SCREEN Holdings
    backlog at record. Signals next capacity expansion wave.
    [recall("full transcript, Japan Chip Equipment") for detail]

NEWS (3 items matched your domain):
  • Reuters: "Samsung to invest $12B in advanced packaging"
  • Nikkei: "Ajinomoto raises ABF substrate prices 15%"
  • Caixin: "SMIC 28nm utilization hits 95%"

MACRO CONTEXT (from Macro agent's last briefing):
  10Y yield: 4.32% (+2bp). Fed funds rate: 4.50% (unchanged).
  Next FOMC: Apr 30.

OPEN THREADS FROM LAST SESSION:
  [pending] Cross-check ASML order backlog vs TSMC capex guide
  [pending] Update HBM supply model with SK Hynix Q1 data

══════════════════════════════════════════════════
```

---

## Cost Estimates

| Source | Daily Cost | Monthly Cost |
|--------|-----------|-------------|
| YouTube RSS + transcripts | ~$0.02 (50 channels, ~20 new videos) | ~$0.60 |
| News RSS feeds | Free | Free |
| FRED API | Free | Free |
| Yahoo Finance / yfinance | Free | Free |
| SEC EDGAR | Free | Free |
| NewsAPI.org | Free (100 req/day) | Free |
| CoinGecko | Free | Free |
| Context processor summarization | ~$0.10/day (cheap model) | ~$3.00 |
| **Total ingestion cost** | **~$0.12/day** | **~$3.60/month** |

Agent reasoning calls (Opus 4.6) are the real cost — estimated $50-100/month for daily meetings at monthly decision cadence. The data pipeline is essentially free.

---

## Implementation Priority

```
Phase 1 (April — single agent MVP):
  [x] FRED API for macro data
  [x] Yahoo Finance for market data
  [x] News RSS feeds (top 5-10 sources)
  [x] YouTube RSS + transcript pipeline (5-10 channels)

Phase 2 (May — multi-agent):
  [ ] SEC EDGAR filing monitor
  [ ] Insider transaction tracking (Form 4)
  [ ] Earnings call transcript pipeline
  [ ] Expanded YouTube channel list
  [ ] Substack/blog RSS feeds

Phase 3 (Summer — enrichment):
  [ ] CFTC COT data
  [ ] 13F institutional holdings
  [ ] Podcast transcription
  [ ] Book knowledge base (parsed + searchable)
  [ ] Historical data archive for base rates

Phase 4 (Future):
  [ ] Satellite / shipping data
  [ ] Alternative data (app downloads, web traffic)
  [ ] Vector DB for semantic search over archives
```

---

## Key Design Decisions

1. **RSS-first for discovery.** Free, no API keys, no quotas. YouTube RSS, news RSS, Substack RSS, podcast RSS. API fallback only when RSS is insufficient.

2. **Context processor (cheap LLM) sits between raw data and agents.** Raw transcripts/articles are 5-15K tokens. Summaries are 200-500 tokens. Agents stay lean. Full text available via `recall()`.

3. **Subscriptions are per-agent and configurable.** Each agent subscribes to sources relevant to their domain. Stored in agent config alongside identity.md. Can evolve over time.

4. **Morning feed is compiled, not streamed.** All ingestion runs overnight. Agents wake to a compiled briefing. No real-time interrupts except priority events (flash crashes, breaking news).

5. **Everything is archived.** Full transcripts, full articles, full filings stored in DB. Agents see summaries in context but can recall full text on demand. This is the engagement ledger pattern from AgentOS.

6. **Local execution for YouTube transcripts.** Cloud IPs get blocked by YouTube. Run the transcript extraction locally on Samuel's machine or a dedicated ingestion server. Agents themselves never touch YouTube directly.
