# Trade Execution & Portfolio Tracking

> How trades get approved, executed, and tracked across all three structures.
> Paper portfolio — no real broker, no real capital. We track entry price,
> exit price, return percentage, and holding period against real market data.

---

## Core Principle: Paper Portfolio, Real Prices

We are NOT routing orders through a broker. The portfolio is a list of
positions with percentage allocations. "Buying" means recording the market
price at approval time. "Selling" means recording the market price at exit.
The return is the difference.

This means we skip: broker integration, order fill simulation, slippage
modeling, commission tracking, cash management. We focus on what matters
for the research: **which topology made better calls, at what confidence,
over what timeframe.**

---

## When Orders Are Placed

| Structure | When | Who Approves | Mechanic |
|-----------|------|-------------|----------|
| Council | Evening meeting | Group vote (≥0.60 weighted) | Agent proposes REC-*, group votes, passed recs become orders |
| Firm | Evening synthesis | CIO (final say) | Sector heads present recs, CIO approves/rejects |
| Model | After signal submission | Optimizer (code) | Agents submit signals, optimizer calculates portfolio |

**The one exception:** Kill conditions on existing positions. If a stop-loss
or kill price is hit, the system auto-executes. This is a pre-committed exit
approved when the original trade was approved. No meeting needed.

---

## Position Lifecycle

```
PROPOSED → APPROVED → EXECUTED → OPEN → CLOSED
    │          │          │         │        │
    │          │          │         │        ├── CLOSED_WIN (profit)
    │          │          │         │        ├── CLOSED_LOSS (loss)
    │          │          │         │        └── KILLED (kill condition met)
    │          │          │
    │          │          └── Entry price recorded (next market open)
    │          │
    │          ├── APPROVED by vote/CIO/optimizer
    │          └── REJECTED (stays in personal track record only)
    │
    └── Agent submits REC-* in evening brief
```

### Position Entry

```
POSITION SCHEMA:
  pos_id:         [auto: POS-{structure}-{YYYYMMDD}-{seq}]
  source_rec_id:  [the REC-* that was approved]
  ticker:         [asset symbol]
  direction:      LONG | SHORT
  size_pct:       [% of portfolio, as proposed by agent]
  entry_price:    [market open price on execution day]
  entry_date:     [date position opened]
  target_price:   [from original rec]
  stop_price:     [from original rec — hard exit]
  kill_condition: [from original rec — qualitative exit]
  kill_metric:    [data source to check]
  thesis_id:      [link to underlying thesis]
  proposed_by:    [agent name]
  status:         OPEN

  # SHORT-specific
  max_loss_pct:   [hard stop, from original rec]
  squeeze_risk:   [LOW/MEDIUM/HIGH at entry]
```

### Position Exit

Positions close when any of these conditions are met:

```
EXIT TRIGGERS (checked daily by system code):
  1. STOP PRICE HIT     — price drops to stop_price (longs)
                           or rises to stop_price (shorts)
                         → auto-close, status: CLOSED_LOSS

  2. TARGET PRICE HIT   — price reaches target_price
                         → auto-close, status: CLOSED_WIN

  3. KILL CONDITION MET  — qualitative condition triggered
                           (detected by verification sub-agent)
                         → flag for review at next evening meeting
                         → agent/group decides to close or hold

  4. AGENT PROPOSES EXIT — agent submits REC-* with action: SELL
                           on an existing position
                         → goes through normal approval process

  5. TIME EXPIRY         — position held past stated timeframe
                           with no thesis update
                         → flag for review ("this was a 3-month
                           trade, it's been 4 months — reassess")

  6. AUTO-REBALANCE      — system trims position to fund a new
                           approved trade (see below)
```

### Position Record (on close)

```
CLOSED POSITION:
  pos_id:         POS-COUNCIL-20260415-01
  ticker:         TSM
  direction:      LONG
  size_pct:       5.0%
  entry_price:    $178.50
  entry_date:     2026-04-15
  exit_price:     $195.20
  exit_date:      2026-06-03
  return_pct:     +9.4%
  holding_days:   49
  exit_trigger:   TARGET_PRICE_HIT
  proposed_by:    @SEMI
  status:         CLOSED_WIN
```

---

## Position Sizing

### Who Decides Size

The proposing agent sets `size_pct` in their REC-* based on conviction.
Sizing is part of the agent's skill — Druckenmiller: "Sizing is 70-80%
of the equation." The group votes on the trade thesis, not the size.
If the vote/CIO approves, the proposer's size stands.

Over time, the track record reveals whether an agent sizes well:
agents who over-size losing trades and under-size winners will show
worse risk-adjusted returns than agents who size appropriately.

### Auto-Rebalance to Fund New Positions

When a new position is approved and there isn't enough free cash:

```
BEFORE:
  TSMC:  20%
  NVDA:  15%
  EPD:   10%
  GLD:    5%
  Cash:   5%    ← only 5% free
  New approved position: ASML at 8%
  Need: 8% - 5% cash = 3% from existing positions

REBALANCE (proportional trim):
  Total to trim: 3% from 50% allocated = 6% haircut each
  TSMC:  20% × 0.94 = 18.8%
  NVDA:  15% × 0.94 = 14.1%
  EPD:   10% × 0.94 =  9.4%
  GLD:    5% × 0.94 =  4.7%
  Cash:   5% + 3% freed = 8% → 0% after buying ASML

AFTER:
  TSMC:  18.8%
  NVDA:  14.1%
  EPD:    9.4%
  GLD:    4.7%
  ASML:   8.0%
  Cash:   0.0%

  All trims logged: "AUTO-REBALANCE: trimmed 6% to fund
  POS-COUNCIL-20260620-01 (ASML)"
```

**Edge case: dust positions.** If a trim would push any position
below 1% of portfolio, sell it entirely instead of holding a
meaningless position. Log as "AUTO-CLOSED: position too small
after rebalance."

**Edge case: 100% allocated.** Same math — trim everything
proportionally. The decision to enter a new position IS the
decision to reduce everything else slightly.

**No agent decision needed.** Auto-rebalance is pure math.
System logs every trim. Track record captures the full history.

---

## Portfolio State

The portfolio is a simple table, updated daily:

```
PORTFOLIO — [Structure Name] — [Date]
═══════════════════════════════════════════════════════════

Starting Capital: $1,000,000 (theoretical, for return calculation)

OPEN POSITIONS:
  | Pos ID  | Ticker | Dir   | Size % | Entry   | Current | Return | Days | Kill Condition       |
  |---------|--------|-------|--------|---------|---------|--------|------|----------------------|
  | POS-001 | TSM    | Long  | 18.8%  | $178.50 | $192.00 | +7.6%  | 35   | TSMC rev declines 2mo|
  | POS-002 | NVDA   | Long  | 14.1%  | $142.00 | $155.00 | +9.2%  | 42   | <$120 for 5 days     |
  | POS-003 | EPD    | Long  |  9.4%  | $29.50  | $34.20  | +15.9% | 28   | Hormuz fully reopens |
  | POS-004 | GLD    | Long  |  4.7%  | $198.00 | $205.00 | +3.5%  | 60   | Real yield > 2%      |
  | POS-005 | ASML   | Long  |  8.0%  | $920.00 | $920.00 | +0.0%  | 0    | <€850 for 3 days     |

CASH: 45.0%

SUMMARY:
  Total allocated: 55.0%
  Portfolio return (inception): +7.2%
  Max drawdown: -2.1%
  Positions: 5 open, 3 closed (2 wins, 1 loss)
  Win rate: 66.7%

CLOSED POSITIONS (recent):
  | Pos ID  | Ticker | Dir   | Entry   | Exit    | Return  | Days | Exit Trigger    |
  |---------|--------|-------|---------|---------|---------|------|-----------------|
  | POS-C01 | TLT    | Long  | $92.00  | $88.50  | -3.8%   | 14   | STOP_PRICE_HIT  |
  | POS-C02 | MSTR   | Short | $185.00 | $142.00 | +23.2%  | 45   | TARGET_HIT      |
  | POS-C03 | PLTR   | Long  | $28.00  | $35.50  | +26.8%  | 62   | AGENT_EXIT      |
```

---

## Pre-Execution Price Check

Orders are approved in the evening meeting (~9-10 PM ET). Markets don't
open until 9:30 AM ET the next day. Overnight, anything can happen.

```
PRE-EXECUTION CHECK (6:00 AM ET, code, no agents)
═══════════════════════════════════════════════════

For each pending order:

  1. Fetch pre-market price (or previous close if pre-market
     not available)

  2. Compare against order limits:

     IF direction = LONG:
       pre_market_price ≤ entry_price × 1.05  → EXECUTE
       pre_market_price > entry_price × 1.05   → HOLD (price moved >5%)
       pre_market_price > kill_price            → CANCEL

     IF direction = SHORT:
       pre_market_price ≥ entry_price × 0.95  → EXECUTE
       pre_market_price < entry_price × 0.95   → HOLD (price moved >5%)
       pre_market_price < stop_price           → CANCEL

  3. Check for material overnight news (keyword scan):
     If major event affecting the ticker detected → HOLD

  4. Results:
     EXECUTE → record market open price as entry_price
     HOLD    → flag for re-evaluation at next evening meeting
     CANCEL  → auto-cancel, log reason, notify agent

The 5% threshold is a sanity check: if the price moved more
than 5% overnight, the trade thesis may be stale. The agent
should re-evaluate rather than blindly enter.
```

---

## Structure-Specific Execution

### Council (Democratic)

```
Agent submits REC-* in evening brief
  → Revealed in evening meeting
  → Group debates
  → >50% signal READY_TO_CLOSE or 100 turns
  → Vote: all 14 agents submit {rec_id, support/oppose, confidence}
  → ≥0.60 weighted: APPROVED → order queued
  → <0.60: REJECTED → personal track record only
  → Risk can veto any approved order (final in Council)
  → 6:00 AM pre-execution check
  → 9:30 AM execute at market open (or hold/cancel)
```

### Firm (Authoritarian)

```
Analyst submits REC-* to sector head by ~8:30 PM
  → Sector head decides whether to escalate
  → If escalated: included in sector head's evening brief
  → Presented in evening synthesis
  → CIO approves or rejects (final say)
  → CIO can override Risk veto (on permanent record)
  → Approved → order queued
  → 6:00 AM pre-execution check
  → 9:30 AM execute at market open (or hold/cancel)
```

### Model (Algorithmic)

```
Agent submits signal (REC-* schema) after morning data ingestion
  → Optimizer collects all 13 signals
  → Weights by believability
  → Manages correlation
  → Enforces risk constraints
  → Outputs target portfolio (list of positions + sizes)
  → Differences from current portfolio → orders queued
  → 6:00 AM pre-execution check
  → 9:30 AM execute at market open (or hold/cancel)
```

---

## The $100 Real Money Parallel

Samuel runs a separate $100 brokerage account as a learning experiment.
This is NOT managed by the system. Samuel manually mirrors select
high-conviction trades from whichever structure he trusts most.

The $100 portfolio is tracked separately for honesty but has zero
influence on the experiment. It exists purely so Samuel has skin in
the game and can write about real P&L in the research paper.

---

## What Gets Stored (Implementation)

For the paper portfolio, all data lives in SQLite (MVP):

```sql
-- Positions table
CREATE TABLE positions (
  pos_id TEXT PRIMARY KEY,
  structure TEXT,           -- 'council', 'firm', 'model'
  source_rec_id TEXT,
  ticker TEXT,
  direction TEXT,           -- 'LONG', 'SHORT'
  size_pct REAL,
  entry_price REAL,
  entry_date TEXT,
  exit_price REAL,
  exit_date TEXT,
  return_pct REAL,
  holding_days INTEGER,
  target_price REAL,
  stop_price REAL,
  kill_condition TEXT,
  kill_metric TEXT,
  thesis_id TEXT,
  proposed_by TEXT,
  exit_trigger TEXT,
  status TEXT               -- 'OPEN', 'CLOSED_WIN', 'CLOSED_LOSS', 'KILLED'
);

-- Portfolio snapshots (daily)
CREATE TABLE portfolio_snapshots (
  snapshot_id TEXT PRIMARY KEY,
  structure TEXT,
  date TEXT,
  total_return_pct REAL,
  cash_pct REAL,
  max_drawdown REAL,
  positions_json TEXT       -- JSON array of current positions
);

-- Orders (pending execution)
CREATE TABLE orders (
  order_id TEXT PRIMARY KEY,
  structure TEXT,
  source_rec_id TEXT,
  ticker TEXT,
  direction TEXT,
  size_pct REAL,
  limit_price REAL,
  kill_price REAL,
  status TEXT,              -- 'QUEUED', 'EXECUTED', 'HELD', 'CANCELLED'
  created_at TEXT,
  executed_at TEXT,
  execution_price REAL,
  pre_check_result TEXT     -- 'EXECUTE', 'HOLD', 'CANCEL'
);

-- Auto-rebalance log
CREATE TABLE rebalances (
  rebalance_id TEXT PRIMARY KEY,
  structure TEXT,
  date TEXT,
  trigger_order_id TEXT,    -- the new order that caused the rebalance
  trims_json TEXT           -- JSON: [{ticker, old_pct, new_pct, reason}]
);
```
