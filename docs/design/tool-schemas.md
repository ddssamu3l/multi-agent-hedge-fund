# Agent Tool Schemas

> Definitions for all tools available to analyst agents. Each tool has
> explicit input parameters, output structure, cost, and usage guidance.
>
> Tools are categorized into always-loaded (used most sessions) and
> searchable (discovered on demand via meta-tool, to save token overhead).
> See `AGENT_ARCHITECTURE_REFERENCE.md` § Tool Architecture.

---

## Tool Categories

```
ALWAYS-LOADED (in every agent's context):
  get_market_data      — stock prices, crypto, macro indicators
  read_analysis        — read another agent's published analysis
  write_analysis       — publish analysis to org feed
  save_memory          — persist to memory.md
  recall               — search memory/archive
  submit_recommendation — structured REC-* output
  submit_prediction    — structured PRED-* output
  end_cycle            — wrap up, commit all buffered actions

SEARCHABLE (discovered via meta-tool, loaded on demand):
  trading_agents_analysis — TradingAgents multi-analyst pipeline
  get_earnings_data       — SEC filings, earnings call transcripts
  get_fred_data           — FRED macro economic series
  web_search              — general web search
  read_document           — read from knowledge base
  request_dm              — initiate private DM
  request_meeting         — initiate multi-agent private meeting
```

---

## Tool: trading_agents_analysis

**Purpose:** Research sub-agent tool. Runs the TradingAgents pipeline
(4 analysts + bull/bear debate + risk debate + portfolio manager) on a
single stock ticker. Returns a multi-perspective analysis with a 5-tier
rating. Use as a sanity check, second opinion, or quick deep-dive on a
specific stock. NOT a decision-maker — just a research instrument.

**Source:** [TauricResearch/TradingAgents](https://github.com/TauricResearch/TradingAgents)
(Apache 2.0, integrated as a dependency)

**Cost:** ~12-15 LLM calls per invocation (cheap model for analysts/debate,
deep model for research manager + portfolio manager). Estimated $0.05-0.15
per call depending on model selection.

**Latency:** 2-5 minutes per call (blocking). Analysts run sequentially,
not in parallel. Two debate phases add additional rounds.

### Input Parameters

```
trading_agents_analysis(
  ticker:     str       — REQUIRED. Stock symbol.
                          Examples: "NVDA", "TSM", "MSTR",
                          "RY.TO" (Toronto), "0700.HK" (Hong Kong)

  date:       str       — OPTIONAL. Analysis date. Defaults to today.
                          Format: "YYYY-MM-DD"
                          Example: "2026-03-30"
                          Note: data is fetched AS OF this date.
                          Future dates will use latest available data.

  analysts:   list[str] — OPTIONAL. Which analyst modules to run.
                          Default: ["market", "social", "news",
                                    "fundamentals"]
                          Valid values:
                            "market"       — technical indicators
                                             (MACD, RSI, Bollinger,
                                             ATR, SMA, EMA, VWMA)
                            "social"       — social media / Reddit /
                                             X sentiment analysis
                            "news"         — recent news, insider
                                             transactions, global events
                            "fundamentals" — balance sheet, cashflow,
                                             income statement, overview
                          Omit any to reduce cost/latency.
                          Example: ["market", "fundamentals"] for a
                          quick technical + financial check (~40% cheaper)

  depth:      str       — OPTIONAL. Debate depth.
                          "quick" (default) — 1 round per debate
                            (2 investment debate turns + 3 risk turns)
                          "deep" — 3 rounds per debate
                            (6 investment turns + 9 risk turns, ~3x cost)
)
```

### Output Structure

```
{
  # ── FINAL VERDICT ──
  rating:              str — "BUY" | "OVERWEIGHT" | "HOLD" |
                              "UNDERWEIGHT" | "SELL"
  thesis:              str — Portfolio manager's investment thesis.
                              1-2 paragraphs. Includes rating rationale,
                              key drivers, risk factors.

  # ── ANALYST REPORTS (one per selected analyst) ──
  market_report:       str — Technical analysis with indicators.
                              Includes markdown table of MACD, RSI,
                              Bollinger Band position, volume analysis,
                              moving average crossovers.
                              (null if "market" not in analysts list)

  sentiment_report:    str — Social media sentiment scores, Reddit/X
                              discussion themes, retail investor
                              sentiment indicators.
                              (null if "social" not in analysts list)

  news_report:         str — Recent news summary, insider transactions,
                              global affairs affecting the stock.
                              (null if "news" not in analysts list)

  fundamentals_report: str — Balance sheet health, cashflow analysis,
                              income statement trends, company overview,
                              key ratios (P/E, EV/EBITDA, FCF yield).
                              (null if "fundamentals" not in analysts list)

  # ── DEBATE TRANSCRIPTS ──
  bull_case:           str — Strongest arguments FOR the investment.
                              Compiled from bull researcher across
                              all debate rounds.

  bear_case:           str — Strongest arguments AGAINST the investment.
                              Compiled from bear researcher across
                              all debate rounds.

  risk_assessment:     str — Summary of aggressive/conservative/neutral
                              risk debate. Includes portfolio manager's
                              final risk judgment.
}
```

### Usage Examples

**Quick sanity check before recommending a stock:**
```
Agent SEMI is forming a thesis on TSMC. Before proposing
BUY TSM to the group, they run a quick check:

  result = trading_agents_analysis(
    ticker="TSM",
    analysts=["market", "fundamentals"],
    depth="quick"
  )

  → result.rating = "BUY"
  → result.fundamentals_report confirms strong FCF and
    revenue growth trajectory
  → SEMI references this in their pre-meeting brief:
    "TradingAgents independently rates TSM as BUY.
    Their fundamental analysis aligns with my thesis
    on CoWoS capacity expansion."
```

**Deep research on a potential short:**
```
Agent CRYPTO suspects MSTR is in Ponzi phase of Minsky
cycle. They want a full multi-perspective analysis:

  result = trading_agents_analysis(
    ticker="MSTR",
    analysts=["market", "social", "news", "fundamentals"],
    depth="deep"
  )

  → result.rating = "UNDERWEIGHT"
  → result.bear_case cites leverage risk and BTC dependency
  → result.bull_case cites BTC long-term appreciation thesis
  → CRYPTO shares the bear_case in a DM to @MACRO:
    "TradingAgents bear case on MSTR aligns with my
    Minsky analysis. They flag the same leverage spiral."
```

**Screening multiple tickers quickly:**
```
Agent MINERALS is exploring copper miners. Runs quick
technical screens on 3 tickers:

  for ticker in ["FCX", "SCCO", "TECK"]:
    result = trading_agents_analysis(
      ticker=ticker,
      analysts=["market"],
      depth="quick"
    )
    # Only dig deeper on stocks with BUY/OVERWEIGHT signal
```

### Usage Guidance

- **Use freely during execution block.** No caps, no gating. Agents
  decide when and whether to use this tool.
- **Not authoritative.** TradingAgents has no voting weight, no
  believability score, no track record in our system. It's a Bloomberg
  terminal, not a colleague.
- **Cost-conscious usage.** Each call is $0.05-0.15. Running it on
  every stock in a sector is fine occasionally but shouldn't be a daily
  habit. Omitting analysts you don't need (e.g., just "fundamentals")
  cuts cost by ~60%.
- **Context management.** Full output is ~2-3K tokens. The viewport
  model collapses it to a one-liner when the agent's attention shifts,
  same as any data source. The engagement ledger preserves the full
  output for end-of-session memory.
- **Track usage in journal.** When an agent invokes this tool, it's
  logged in their daily journal. Over time, this reveals patterns:
  which agents rely on it, which ignore it, whether its signals
  correlate with better or worse outcomes.

### Implementation Notes

```python
# Wrapper around TradingAgents library
from tradingagents.graph.trading_graph import TradingAgentsGraph

def trading_agents_analysis(
    ticker: str,
    date: str = None,
    analysts: list[str] = None,
    depth: str = "quick"
) -> dict:
    config = {
        "llm_provider": "anthropic",
        "deep_think_llm": "claude-sonnet-4-6",
        "quick_think_llm": "claude-haiku-4-5",
        "max_debate_rounds": 1 if depth == "quick" else 3,
        "max_risk_discuss_rounds": 1 if depth == "quick" else 3,
    }

    ta = TradingAgentsGraph(
        selected_analysts=analysts or [
            "market", "social", "news", "fundamentals"
        ],
        config=config
    )

    if date is None:
        date = datetime.now().strftime("%Y-%m-%d")

    final_state, decision = ta.propagate(ticker, date)

    return {
        "rating": decision,
        "thesis": final_state.get("final_trade_decision", ""),
        "market_report": final_state.get("market_report"),
        "sentiment_report": final_state.get("sentiment_report"),
        "news_report": final_state.get("news_report"),
        "fundamentals_report": final_state.get("fundamentals_report"),
        "bull_case": final_state.get(
            "investment_debate_state", {}
        ).get("bull_history", ""),
        "bear_case": final_state.get(
            "investment_debate_state", {}
        ).get("bear_history", ""),
        "risk_assessment": final_state.get(
            "risk_debate_state", {}
        ).get("judge_decision", ""),
    }
```

---

## Tool: get_market_data

**Purpose:** Fetch stock prices, crypto prices, and macro indicators.
Always-loaded. The most frequently used tool.

```
get_market_data(
  ticker:     str       — Stock/crypto symbol ("NVDA", "BTC-USD")
  data_type:  str       — "price" | "technical" | "overview"
  period:     str       — "1d" | "5d" | "1mo" | "3mo" | "6mo" | "1y"
)

Returns: {
  price:      float     — current/latest price
  change_pct: float     — % change over period
  volume:     int       — trading volume
  high:       float     — period high
  low:        float     — period low
  technicals: dict      — if data_type="technical": RSI, MACD, etc.
  overview:   dict      — if data_type="overview": market cap, P/E, etc.
}
```

---

## Tool: get_fred_data

**Purpose:** Fetch FRED macro economic data series. Searchable (loaded
on demand). Used primarily by MACRO, LIQUIDITY, and JAPAN agents.

```
get_fred_data(
  series_id:  str       — FRED series ID ("UNRATE", "DGS10", "CPIAUCSL")
  start_date: str       — "YYYY-MM-DD"
  end_date:   str       — "YYYY-MM-DD" (defaults to today)
)

Returns: {
  series_id:  str
  values:     list[{date: str, value: float}]
  latest:     float     — most recent value
  change:     float     — change from previous reading
  metadata:   dict      — series description, frequency, units
}
```

---

## Tool: web_search

**Purpose:** General web search. Searchable. For finding news, research,
data not available through structured APIs.

```
web_search(
  query:      str       — search query
  num_results: int      — max results to return (default 5)
  recency:    str       — "day" | "week" | "month" | "any"
)

Returns: {
  results: list[{
    title:    str
    url:      str
    snippet:  str
    date:     str
  }]
}
```

---

## Tool: submit_recommendation

**Purpose:** Submit a structured recommendation (REC-* schema). Always-loaded.
This is how agents make trade proposals. System code auto-creates a track
record entry from this output.

See `docs/design/runtime-documents.md` for the full REC-* schema.

```
submit_recommendation(
  action:         str   — "BUY" | "SELL" | "SHORT" | "HOLD" | "HEDGE" | "WATCH"
  asset:          str   — ticker symbol
  asset_name:     str   — full company name
  size_pct:       float — suggested % of portfolio
  confidence:     float — 0.0-1.0
  timeframe:      str   — expected holding period
  entry_price:    float — target entry (null if WATCH)
  target_price:   float — price target
  stop_price:     float — hard exit price
  thesis_id:      str   — link to active thesis
  variant_view:   str   — how this differs from consensus
  reasoning:      str   — causal chain from finding to trade
  kill_condition:  str  — measurable invalidation condition
  kill_metric:    str   — data source to check
  counterparty:   str   — who disagrees and why
  tags:           list  — domain tags

  # SHORT-specific (required when action = "SHORT")
  max_loss_pct:   float — hard stop loss percentage
  catalyst:       str   — specific event/timeline
  squeeze_risk:   str   — "LOW" | "MEDIUM" | "HIGH"
  minsky_stage:   str   — "HEDGE" | "SPECULATIVE" | "PONZI"
)

Returns: {
  rec_id:         str   — auto-assigned REC-{agent}-{date}-{seq}
  status:         str   — "PENDING"
  created_at:     str   — timestamp
}
```

---

## Tool: submit_prediction

**Purpose:** Submit a structured prediction (PRED-* schema). Always-loaded.
System code auto-creates a track record entry.

See `docs/design/runtime-documents.md` for the full PRED-* schema.

```
submit_prediction(
  question:       str   — specific, time-bounded, resolvable
  resolution_date: str  — when this can be graded
  probability:    float — 0.0-1.0
  direction:      str   — "BULLISH" | "BEARISH" | "NEUTRAL"
  asset:          str   — ticker or domain ("MACRO", "GEO")
  reasoning:      str   — why this probability
  key_assumptions: list — what must hold
  tags:           list  — domain tags for calibration analysis
)

Returns: {
  pred_id:        str   — auto-assigned PRED-{agent}-{date}-{seq}
  status:         str   — "OPEN"
  created_at:     str   — timestamp
}
```

---

## Tool: request_dm

**Purpose:** Initiate a private 1-on-1 DM with another agent. Searchable.
The recipient must accept before the conversation starts.

```
request_dm(
  to:         str       — agent name ("@MACRO", "@SEMI")
  topic:      str       — one-line topic ("Hormuz impact on crude imports")
)

Returns: {
  dm_id:      str       — auto-assigned
  status:     str       — "PENDING_ACCEPT" | "ACCEPTED" | "DECLINED"
}
```

---

## Tool: request_meeting

**Purpose:** Initiate a multi-agent private meeting. Searchable.
Each invitee independently accepts or declines.

```
request_meeting(
  invitees:   list[str] — agent names (["@CHINA", "@OIL", "@GEOPOLITICS"])
  topic:      str       — one-line topic ("Hormuz scenario analysis")
)

Returns: {
  meeting_id: str       — auto-assigned
  accepted:   list[str] — agents who accepted
  declined:   list[str] — agents who declined
  status:     str       — "ACTIVE" | "CANCELLED" (if all declined)
}
```

---

## Tool Access by Structure

| Tool | Council (A) | Firm (B) | Model (C) |
|------|:-----------:|:--------:|:---------:|
| get_market_data | all | all | all |
| trading_agents_analysis | all | all | all |
| web_search | all | all | all |
| get_fred_data | all | all | all |
| get_earnings_data | all | all | all |
| read_analysis | all | within sector + evening | none |
| write_analysis | all | to sector feed | to optimizer |
| submit_recommendation | all | all | all |
| submit_prediction | all | all | all |
| request_dm | all | all (cross-sector ok) | none |
| request_meeting | all | all | none |
| save_memory | all | all | all |
| recall | all | all | all |

Note: Structure C agents have no communication tools. They produce
signals in isolation. Structure B agents can DM across sectors but
don't see other sectors' published analyses by default.
