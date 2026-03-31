# Organizational Structures Design

> Three organizational topologies running simultaneously against the same data,
> same agents, same domain knowledge. The ONLY variable is topology.
> This is the core experiment.

---

## The Three Structures

```
STRUCTURE A          STRUCTURE B              STRUCTURE C
"THE COUNCIL"        "THE FIRM"               "THE MODEL"
(Peer Network)       (4-Layer Pyramid)        (Unified Synthesis)

 Everyone sees        Progressive filtering    No interaction.
 everything.          at each layer.           Pure independent
 Weighted consensus.  CIO decides.             signals → algorithm.

   ┌─┐ ┌─┐ ┌─┐       ┌─────┐                 ┌─┐ ┌─┐ ┌─┐
   │A│ │A│ │A│        │RISK │ L4              │A│ │A│ │A│
   └┬┘ └┬┘ └┬┘        └──┬──┘                 └┬┘ └┬┘ └┬┘
    │   │   │          ┌──▼──┐                  │   │   │
    ├───┼───┤          │ CIO │ L3               ├───┼───┤
    │   │   │          └──┬──┘                  │   │   │
   ┌▼───▼───▼┐       ┌───┼───┐              ┌──▼───▼───▼──┐
   │  RISK   │       │SH │SH │SH  L2        │  OPTIMIZER  │
   │ (veto)  │       └┬──┘┬──┘┬┘             │  (code)     │
   └────┬────┘      ┌─┼───┼───┼─┐            └─────────────┘
        │           │A│A│A│A│A│A│  L1
   ┌────▼────┐      └─┘─┘─┘─┘─┘─┘
   │ WEIGHTED│
   │ SYNTH   │
   └─────────┘
```

---

## Structure A: "The Council" (Peer Network)

**Inspired by:** Tiger Cubs idea-sharing + NAIC investment clubs +
GJP superforecaster teams + our collab_prototype experiment

### Philosophy
No hierarchy. All analysts are peers. Each brings deep domain
expertise and a committed perspective. Objectivity emerges from
the collision of multiple perspectived analyses. Decisions by
believability-weighted vote.

### Agent Roster

**Domain Analysts (11 broad generalists):**

| Agent | Domain | Scope |
|-------|--------|-------|
| SEMI | Semiconductors | Fabs, equipment, memory, packaging, entire supply chain |
| TECH | Technology | Software, cloud, AI companies, enterprise, consumer tech |
| CRYPTO | Crypto & Digital Assets | BTC, ETH, DeFi, on-chain, stablecoins, regulatory |
| MACRO | Macro & Monetary Policy | Fed/ECB/BoJ/PBoC, CPI, employment, fiscal policy, rates |
| CREDIT | Credit & Financial System | Yield curves, IG/HY spreads, bank lending, shadow banking, gold as monetary signal, bank earnings, financial system stress, mortgage markets |
| JAPAN | Japan | BoJ, YCC, yen carry trade, JGB market, demographics |
| CHINA | China | PBoC, 土地财政, dual circulation, trade, HK, Taiwan risk |
| OIL | Energy | OPEC+, crude, refining, LNG, shipping lanes, energy transition |
| GEOPOLITICS | Geopolitics | US foreign policy, sanctions, NATO, conflicts, defense, global chokepoints (Hormuz, Suez, Taiwan Strait, Malacca), Middle East |
| MINERALS | Commodities | Copper, gold, silver, lithium, rare earths, mining, commodity cycles |
| TRADE_FLOWS | Global Commerce | Shipping (Baltic Dry Index, container rates), port congestion, trade volumes, supply chain disruptions, inventory cycles, freight routing, Suez/Panama traffic, global manufacturing PMI |

**Cross-Cutting Functional Specialists (2):**

| Agent | Function | Scope |
|-------|----------|-------|
| TECHNICAL | Technical Analysis | Charts, indicators, price action, volume across ALL sectors. RSI, MACD, Bollinger, moving averages, support/resistance. The "I don't care about your narrative, the chart says X" agent. |
| SENTIMENT | Sentiment Analysis | Social media, Reddit/X, retail flows, options flow (put/call ratios), short interest, insider transactions across ALL sectors. The "retail is piling into X" or "insiders are dumping Y" agent. |

**Risk (1):**

| Agent | Role | Scope |
|-------|------|-------|
| RISK | Referee (veto power) | Cross-cutting: correlation, concentration, drawdown, Minsky stage. Sees all DMs. Can veto any trade. Veto is FINAL in the Council (no CIO to override). |

**Total: 14 agents** (11 domain + 2 cross-cutting + 1 risk referee)

**Why 14?** Peer networks break down beyond ~15 people. Real investment clubs (NAIC) are 10-20 members. GJP superforecaster teams are ~12. This is the sweet spot for flat peer debate. The Firm scales to 37 agents because hierarchy manages information flow — the Council deliberately stays small because that's its natural operating point.

### Information Architecture
- **Full transparency among peers.** Every agent can see every other
  agent's published analysis and positions.
- **Private DMs allowed.** Any agent can DM any other agent. DMs are
  invisible to the group. This creates information asymmetry and
  enables private coalitions (proven most effective anti-convergence
  mechanism from collab experiment).
- **Risk agent sees everything** including DMs (like Citadel's central
  risk team). Risk is the only entity with complete information.

### Decision Mechanism
- Each agent produces a structured position:
  `{ticker, direction, confidence, timeframe, kill_condition, reasoning}`
- Positions aggregated via **believability-weighted vote:**
  - Each agent's weight = track record accuracy on THIS TYPE of call
  - Macro agent's weight is high on rate predictions, low on chip stocks
  - Weights recalculated monthly based on rolling prediction accuracy
- **Extremized mean** (from GJP research): After aggregation, push the
  consensus probability further from 50% by a calibrated factor.
  Independent convergence (3+ agents reaching same conclusion through
  different paths) triggers this amplification.
- **Risk veto:** Risk agent can block any position with stated reasoning.
  To override a risk veto, 70%+ weighted vote is required (supermajority).

### Meeting Format
- **Daily morning round-table** (~30 turns):
  Topic as suggestion, not assignment ("someone floated: China PMI
  came in weak, what does this mean for the portfolio?")
  Weighted random speaker selection (some agents speak rarely but
  with high impact). DMs active during meeting.
- **Monthly allocation meeting** (~50 turns):
  Formal position presentations. Each agent presents their strongest
  conviction with full reasoning, kill condition, and counterparty
  analysis. Followed by open debate. Vote at end.
- **Spontaneous bilateral meetings:**
  Any agent can request a private meeting with any other agent.
  The other agent can accept or decline. These are DM conversations
  outside the group context.

### What This Tests
- Does full information access produce better decisions or drown
  agents in noise?
- Does peer debate surface cross-domain connections (Ajinomoto-style)
  that hierarchical filtering would miss?
- Does consensus decision-making lead to mediocre middle-ground
  positions, or does believability weighting preserve conviction?
- Do DM-driven coalitions improve or degrade group decision quality?

---

## Structure B: "The Firm" (4-Layer Pyramid)

**Inspired by:** Point72 analyst pipeline + Citadel sector pods +
Bridgewater believability weighting + traditional finance hierarchy

### Philosophy
Progressive information filtering. Each layer sees less raw data
but more synthesis. The CIO operates on refined intelligence, not
raw noise. A Risk Committee with full visibility serves as the
ultimate check. Signal is extracted from noise at each layer, but
each layer also risks filtering out genuine signal.

### Agent Roster

**Layer 1 — Domain Analysts (27 narrow specialists)**

Semiconductors (→ SEMI_HEAD):

| Agent | Domain |
|-------|--------|
| FABS | TSMC, Samsung Foundry, Intel Foundry, GlobalFoundries |
| EQUIPMENT | ASML, LAM Research, Applied Materials, Tokyo Electron |
| MEMORY | SK Hynix, Micron, Samsung Memory, WD/Kioxia |
| PACKAGING | ASE, Amkor, Ajinomoto ABF substrates, CoWoS |

Technology (→ TECH_HEAD):

| Agent | Domain |
|-------|--------|
| AI_INFRA | NVIDIA, AMD, datacenter GPUs, custom silicon (TPU, Trainium) |
| CLOUD | AWS, Azure, GCP, hyperscaler capex, datacenter REITs |
| ENTERPRISE | SaaS, Oracle, SAP, enterprise AI adoption |

Macro & Rates (→ MACRO_HEAD):

| Agent | Domain |
|-------|--------|
| US_MACRO | Fed policy, CPI, employment, fiscal, housing |
| GLOBAL_RATES | ECB, PBoC divergence, carry trades, FX |
| CREDIT | Yield curves, IG/HY spreads, bank lending, shadow banking, gold as monetary signal |
| JAPAN | BoJ, YCC, yen carry trade, JGB market, demographics |
| FINANCIALS | Bank earnings, loan growth, financial system stress, mortgage markets |

Energy & Power (→ ENERGY_HEAD):

| Agent | Domain |
|-------|--------|
| OIL | OPEC+, crude, refining, shipping lanes, LNG |
| POWER | Nuclear/SMR, renewables, grid infrastructure, utility capex |
| PIPELINES | Midstream operators, LNG terminals, energy infrastructure |

Geopolitics & Trade (→ GEO_HEAD):

| Agent | Domain |
|-------|--------|
| CHINA | PBoC, 土地财政, dual circulation, HK, Taiwan risk |
| US_FOREIGN | Sanctions, NATO, alliances, defense spending, AUKUS |
| EMERGING | India, SE Asia, Middle East, Africa (commodity demand) |
| TRADE | Tariffs, export controls, supply chain reshoring, CHIPS Act |
| TRADE_FLOWS | Global shipping (Baltic Dry Index, container rates), port congestion, trade volumes, supply chain disruptions, inventory cycles, freight routing, Suez/Panama traffic, global manufacturing PMI |

Commodities (→ COMMODITIES_HEAD):

| Agent | Domain |
|-------|--------|
| METALS | Gold, silver, copper, aluminum, steel |
| BATTERY | Lithium, cobalt, nickel, graphite (EV + storage) |
| RARE_EARTHS | Processing monopoly, China controls, Western alternatives |

Crypto (→ CRYPTO_HEAD):

| Agent | Domain |
|-------|--------|
| CRYPTO_MACRO | BTC, ETH, regulatory landscape, institutional adoption |
| DEFI | On-chain analytics, DeFi protocols, stablecoins |

Market Intelligence (→ INTEL_HEAD):

| Agent | Domain |
|-------|--------|
| TECHNICAL | Charts, indicators, price action, volume across ALL sectors |
| SENTIMENT | Social media, Reddit/X, retail flows, options flow, insider txns |

**Layer 2 — Sector Heads (8 pure managers, no research)**

| Agent | Sector | Manages |
|-------|--------|---------|
| SEMI_HEAD | Semiconductors | FABS, EQUIPMENT, MEMORY, PACKAGING |
| TECH_HEAD | Technology | AI_INFRA, CLOUD, ENTERPRISE |
| MACRO_HEAD | Macro & Rates | US_MACRO, GLOBAL_RATES, CREDIT, JAPAN, FINANCIALS |
| ENERGY_HEAD | Energy & Power | OIL, POWER, PIPELINES |
| GEO_HEAD | Geopolitics & Trade | CHINA, US_FOREIGN, EMERGING, TRADE, TRADE_FLOWS |
| COMMODITIES_HEAD | Commodities | METALS, BATTERY, RARE_EARTHS |
| CRYPTO_HEAD | Crypto | CRYPTO_MACRO, DEFI |
| INTEL_HEAD | Market Intelligence | TECHNICAL, SENTIMENT |

Sector heads are pure managers — they do no independent research.
They synthesize, filter, assign directives, present upward, and
relay CIO decisions downward.

**Layer 3 — CIO (the strategist)**

| Agent | Role |
|-------|------|
| CIO | Sets strategic priorities. Approves/rejects trades (final say). Silent in standups, speaks only in evening synthesis. Can override Risk veto (on permanent record). |

**Layer 4 — Risk Committee (the check)**

| Agent | Role |
|-------|------|
| RISK | Sees ALL layers unfiltered — all DMs, all reports, all decisions. Veto power. CIO can override but it goes on the permanent record and gets graded in the track record. |

**Total: 37 agents** (27 L1 analysts + 8 sector heads + 1 CIO + 1 risk)

**Why 37?** Hierarchy's advantage is scale. The Council caps at 14
because flat peer networks break down beyond ~15 people. The Firm
handles 27 narrow specialists because the hierarchy manages information
flow — each morning standup is only 3-5 people. The question: does a
hierarchy of 27 narrow specialists outperform 14 broad generalists
debating as peers?

### Layer Responsibilities

**Layer 1 analysts:**
- Do the actual research: read filings, pull data, watch YouTube
  transcripts, analyze earnings calls, form domain-specific views
- Execute CIO directives relayed through sector heads (priority work)
- Can pursue personal research AFTER directives are complete
- Produce structured reports and recommendations for their sector head
- Have deep but narrow vision — they know their domain cold but
  don't see the full portfolio picture
- Can DM any agent in any sector (info barriers are about default
  context, not communication bans)
- NEVER attend the evening synthesis — learn about CIO decisions
  the next morning through their sector head (one-day delay)

**Layer 2 sector heads (pure managers):**
- Do NO independent research
- RELAY CIO directives downward as specific task assignments
- Synthesize analyst findings and FILTER for the CIO — not everything
  goes up. Sector heads own this judgment call. Core hierarchy test:
  does filtering help (removes noise) or hurt (loses signal)?
- Push BACK to analysts: "your thesis contradicts FABS' data, reconcile"
- Relay CIO decisions downward — choose HOW to frame them (verbatim,
  with context, or editorialized). Both upward and downward filtering
  are part of the hierarchy experiment.
- Can DM other sector heads (cross-sector intelligence sharing)
- Attend evening synthesis with CIO and Risk

**Layer 3 CIO:**
- Sets STRATEGIC PRIORITIES for the entire organization daily
  ("I want the tech sector to stress-test semi holdings against
  a rate hike scenario. Macro team: track the yen carry unwind.")
- Only sees sector-level synthesis, NOT raw analyst output
- Makes cross-sector connections: Tech Head says "GPU demand insane" +
  Macro Head says "rates about to rise" = CIO sees the collision
- Approves/rejects trade recommendations (final say)
- Silent in morning standups (prevents HiPPO anchoring)
- Speaks only in evening synthesis
- Can override Risk veto (goes on permanent record, graded in track record)

**Layer 4 Risk Committee:**
- Sees ALL layers: raw analyst data + sector synthesis + CIO decisions
- The ONLY entity with the complete unfiltered picture
- Present in every meeting (standups + evening), observing
- Speaks on every trade recommendation
- Can block CIO decisions with stated reasoning
- Monitors: concentration, correlation, Minsky stage, crack signals,
  drawdown limits, sector exposure

### Information Architecture
```
                    WHAT EACH LAYER SEES
   ═══════════════════════════════════════════════

   Layer 1 analysts:
     ✓ Own domain data (full depth)
     ✓ Other analysts in same sector (standup)
     ✓ Can DM any agent in any sector
     ✗ Other sectors' published analyses (by default)
     ✗ Evening synthesis (never attend)
     ✗ CIO decisions (until relayed next morning by sector head)

   Layer 2 sector heads:
     ✓ Their analysts' full output
     ✓ Other sector heads (evening synthesis + DMs)
     ✓ CIO priorities and decisions (evening synthesis)
     ✗ Other sectors' raw analyst data

   Layer 3 CIO:
     ✓ All sector head synthesis (evening)
     ✓ Listens to all standups (silent)
     ✓ Historical decisions + outcomes
     ✗ Raw analyst data (unless requested via sector head)

   Layer 4 Risk Committee:
     ✓ EVERYTHING (all layers, all standups, all DMs, all data)
```

### Decision Flow (Daily Cycle)
```
EVENING: CIO decides trades + sets tomorrow's strategic priorities
              ↓ (strategy flows DOWN)
MORNING: Sector heads relay directives to analysts as task assignments
              ↓ (analysts EXECUTE directives)
DAY:     Analysts work assigned tasks first, personal research second
              ↓ (research flows UP)
EVENING: Sector heads synthesize + present to CIO. CIO decides.
         Analysts never see this meeting. Cycle repeats.
```

### Meeting Format

See `docs/design/meeting-protocols.md` for full meeting protocol with
phase structure, speaking order, and turn management.

- **Daily morning sector standups** (8 parallel meetings):
  Sector head relays CIO directives → analyst briefs revealed →
  analysts report (juniors first) → sector head synthesizes and
  assigns work. CIO listens silently. Risk observes.
- **Daily evening synthesis** (sector heads + CIO + Risk only):
  Sector heads present findings → cross-sector debate → CIO
  approves/rejects recs → CIO sets tomorrow's priorities.
  L1 analysts are excluded.
- **Monthly Idea Dinner** (~40 turns):
  CIO + all 3 sector heads. Each sector head presents their
  strongest conviction and biggest concern. CIO interrogates.
  Open cross-sector debate. CIO drafts allocation.
- **Monthly Risk Review** (~20 turns):
  Risk Committee reviews CIO's allocation against all raw data.
  Approves, blocks, or flags specific concerns.

### What This Tests
- Does a hierarchy of 27 narrow specialists outperform 14 broad
  generalists debating as peers?
- Does progressive filtering remove noise or lose signal?
- Does the CIO miss cross-domain connections that only exist
  at the ground floor (the Ajinomoto problem)?
- Does top-down priority setting react faster to changing conditions,
  or does it create blind spots where nobody's looking?
- When an analyst finds something important that contradicts the CIO's
  directive, does it reach the CIO? Or does the sector head filter it out?
- Does the one-day information delay on org decisions hurt analyst
  performance, or does it keep them focused on deep domain work?
- Does the Risk Committee's full visibility compensate for the
  CIO's filtered view?

---

## Structure C: "The Model" (Unified Synthesis)

**Inspired by:** Renaissance Technologies' single model +
Prediction markets + Two Sigma's systematic approach

### Philosophy
Remove social dynamics entirely. Each agent is an independent
signal generator that never sees other agents' outputs. A
deterministic algorithm (not an LLM) combines all signals,
manages correlation, and sizes positions. The hypothesis: removing
social influence produces cleaner, more independent signals that
combine better mathematically than socially.

### Agent Roster

**Same agents as the Council** — same broad generalist coverage,
same analytical DNA. The ONLY difference is: no interaction.

| Agent | Domain | Role |
|-------|--------|------|
| SEMI | Semiconductors (full supply chain) | Independent signal generator |
| TECH | Software, cloud, AI companies, enterprise | Independent signal generator |
| CRYPTO | Digital assets, DeFi, on-chain, regulatory | Independent signal generator |
| MACRO | Fed/ECB/BoJ/PBoC, CPI, employment, rates | Independent signal generator |
| CREDIT | Yield curves, spreads, banks, financial system, gold | Independent signal generator |
| JAPAN | BoJ, YCC, yen carry trade, JGB, demographics | Independent signal generator |
| CHINA | PBoC, 土地财政, dual circulation, trade, Taiwan risk | Independent signal generator |
| OIL | OPEC+, crude, refining, LNG, energy transition | Independent signal generator |
| GEOPOLITICS | Foreign policy, sanctions, conflicts, chokepoints, Middle East | Independent signal generator |
| MINERALS | Copper, gold, silver, lithium, rare earths, mining | Independent signal generator |
| TRADE_FLOWS | Global shipping, port congestion, trade volumes, freight | Independent signal generator |
| TECHNICAL | Charts, indicators, price action across all sectors | Independent signal generator |
| SENTIMENT | Social media, retail flows, options flow, insider txns | Independent signal generator |

**Total: 13 agents** + portfolio optimizer (code, not agent)

No risk agent. Risk is embedded as hard-coded constraints in the optimizer.
The Model is the control group for the Council — same agents, same data,
but zero social interaction. If the Council outperforms the Model, the
value of peer debate is proven. If the Model outperforms, social dynamics
are net-negative noise.

### Information Architecture
- **Complete information barriers.** No agent sees any other agent's
  output, positions, or analysis. Ever.
- **Each agent sees only:** their own domain data, their own memory,
  their own track record, and the portfolio's aggregate performance
  (not individual agent contributions).
- **No meetings. No DMs. No debate.**
- This is the experimental control group: what happens when you
  remove social dynamics entirely?

### Signal Format
Each agent produces structured signals on a daily basis:

```
SIGNAL {
  agent:          "SEMI"
  date:           "2026-03-22"
  ticker:         "TSM"
  direction:      "LONG"
  confidence:     0.82          # 0.0 to 1.0
  timeframe:      "3 months"
  size_suggestion: 0.05         # % of portfolio
  kill_condition: "TSMC monthly revenue declines 2 consecutive months"
  reasoning:      "CoWoS capacity booked through 2027, advanced node
                   utilization at 100%, Arizona delay = more Taiwan
                   dependency = geopolitical premium justified"
  tags:           ["semiconductor", "AI", "supply_chain"]
}
```

### Portfolio Optimizer (Deterministic Code)
Not an LLM. A rules-based system that:

1. **Collects all signals** from all agents daily
2. **Weights signals by:**
   - Agent's historical accuracy on this type of call
     (believability weighting, same as Structure A)
   - Signal confidence level
   - Signal agreement across agents (independent convergence bonus)
3. **Manages correlation:**
   - If 5 agents all suggest long semiconductor positions, the
     optimizer recognizes this as one correlated bet, not five
     independent ones
   - Reduces individual position sizes to keep total sector
     exposure within limits
4. **Enforces risk constraints:**
   - Max single position: 10% of portfolio
   - Max sector exposure: 30% of portfolio
   - Max drawdown trigger: -15% → reduce all positions by 50%
   - Correlation limit: no two positions with >0.7 correlation
     can exceed 15% combined
5. **Outputs final portfolio:**
   - Position list with sizes
   - Full audit trail (which signals contributed, weights applied)

### Decision Cadence
- **Daily:** Agents produce signals after morning data ingestion
- **Daily:** Optimizer recalculates optimal portfolio
- **Monthly:** Optimizer executes rebalancing trades
  (daily recalculation but monthly execution to reduce churn
  and align with the "decide slowly" principle)
- **Quarterly:** Believability weights recalculated based on
  rolling prediction accuracy

### What This Tests
- Does removing social dynamics produce cleaner, more independent
  signals?
- Does algorithmic synthesis outperform both peer debate (A) and
  hierarchical filtering (B)?
- Is the Renaissance model (combine many weak signals) superior
  to the Bridgewater model (debate + weighted consensus)?
- What is lost when agents can't challenge each other's assumptions?
  (The counterargument: what is gained when agents aren't anchored
  by each other's biases?)

---

## Comparison Framework

All three structures run simultaneously against the same:
- Market data
- News feeds
- YouTube subscriptions
- Agent domain knowledge (same system prompt, same axioms, same reasoning rules)
- Starting portfolio ($1,000,000 simulated)
- Time period

### Metrics Tracked Per Structure

| Metric | Description |
|--------|-------------|
| Total return | Cumulative P&L |
| Risk-adjusted return (Sharpe) | Return per unit of volatility |
| Max drawdown | Worst peak-to-trough decline |
| Win rate | % of closed positions that were profitable |
| Average conviction accuracy | Do high-confidence calls outperform low-confidence? |
| Signal independence | Correlation between agents' signals (C should be lowest) |
| Prediction journal accuracy | % of timestamped predictions that proved correct |
| Time to insight | How quickly does the structure identify a new opportunity? |
| Information loss metric | Comparing ground-floor analyst output to final decision — what was filtered out? (B only) |
| Cross-domain connection rate | How often do agents identify connections across domains? (A should be highest, C should be zero) |

### Cost Per Structure (Estimated Monthly)

| Structure | Agents | Daily Meetings | Research/Execution | Data Ingestion | Total |
|-----------|--------|---------------|-------------------|----------------|-------|
| A (Council) | 14 | ~$50-70 (morning + evening, 14 agents) | ~$20-30 | ~$4 | ~$75-105 |
| B (Firm) | 37 | ~$60-90 (8 standups + evening synthesis) | ~$80-120 (27 analysts researching) | ~$4 | ~$145-215 |
| C (Model) | 13 | $0 (no meetings) | ~$10-15 (signal generation only) | ~$4 | ~$15-20 |
| **Combined** | | | | | **~$235-340/month** |

The Firm is the most expensive because it has the most agents, but
per-analyst cost is lower than the Council (analysts only attend small
standups, never the evening debate). Structure C is dramatically cheaper
because agents never talk to each other. If C outperforms A and B,
that's a finding worth the price difference.

---

## Agent Identity Differences Across Structures

The same domain agents exist in all three structures, but their
identity.md and system prompt Layer 2 differ:

**Structure A (Council):**
- Identity emphasizes: peer relationships, debate skills, persuasion,
  willingness to challenge, coalition-building
- Knows: "You are an equal voice. Your job is to convince your peers."

**Structure B (Firm):**
- L1 Identity emphasizes: deep research, reporting clarity, responsiveness
  to sector head direction, thoroughness
- L2 Identity emphasizes: synthesis skill, filtering judgment, managing
  analysts, presenting to CIO
- CIO Identity emphasizes: strategic thinking, interrogation, cross-sector
  pattern recognition, decisiveness
- Knows: "You report to [sector head]. Your job is to produce the best
  research your sector head has ever seen."

**Structure C (Model):**
- Identity emphasizes: independent thinking, analytical precision,
  calibrated confidence, no social awareness
- Knows: "You are an independent signal generator. You never see other
  analysts' work. Your signals are combined algorithmically."

This means the "same" SEMI agent actually has three slightly different
identities across the three structures — same domain knowledge, same
reasoning rules, same analytical DNA, but different social orientation.

---

## Sector Groupings (Structure B)

8 sectors modeled on real fund coverage (Point72, Citadel, Balyasny).

### Semiconductors (SEMI_HEAD → 4 analysts)
FABS, EQUIPMENT, MEMORY, PACKAGING

**Why:** The semiconductor supply chain is a single interconnected system.
Fab capacity → equipment lead times → memory allocation → packaging
bottlenecks. The sector head sees the full pipeline and spots cascading
constraints (e.g., CoWoS packaging shortage limits even when fab capacity
is available).

### Technology (TECH_HEAD → 3 analysts)
AI_INFRA, CLOUD, ENTERPRISE

**Why:** AI demand drives GPU demand drives cloud capex drives enterprise
adoption. The sector head connects: NVIDIA earnings → hyperscaler capex
guidance → SaaS company AI feature revenue. Demand signal propagation.

### Macro & Rates (MACRO_HEAD → 5 analysts)
US_MACRO, GLOBAL_RATES, CREDIT, JAPAN, FINANCIALS

**Why:** The largest sector because monetary policy, credit conditions,
currency markets, and the banking system are deeply interlinked. BoJ moves
affect yen carry → global liquidity → risk assets. Fed hikes → banks
tighten → credit spreads widen → everyone feels it. The sector head sees
the full monetary transmission mechanism.

### Energy & Power (ENERGY_HEAD → 3 analysts)
OIL, POWER, PIPELINES

**Why:** Energy supply chain from extraction through generation to
distribution. Oil price → power costs → datacenter economics →
semiconductor demand. The energy transition (nuclear/SMR, renewables)
creates new bottlenecks and investment opportunities.

### Geopolitics & Trade (GEO_HEAD → 5 analysts)
CHINA, US_FOREIGN, EMERGING, TRADE, TRADE_FLOWS

**Why:** Geopolitics drives resource flows, trade policy, and supply chain
routing. China's economy drives commodity demand. US sanctions reshape
supply chains. TRADE_FLOWS monitors the physical movement of goods —
leading indicator for supply disruptions that hit every other sector.

### Commodities (COMMODITIES_HEAD → 3 analysts)
METALS, BATTERY, RARE_EARTHS

**Why:** Raw materials are the foundation layer. Gold/silver as monetary
signals (tracked jointly with CREDIT in Macro). Lithium/cobalt for EV
and storage. Rare earths as China's geopolitical leverage. Commodity
super-cycles drive multi-year investment themes.

### Crypto (CRYPTO_HEAD → 2 analysts)
CRYPTO_MACRO, DEFI

**Why:** Crypto sits at the intersection of technology (mining hardware,
blockchain), macro (risk appetite, dollar alternatives), and regulation.
Small team but high signal — BTC correlation to risk assets is a macro
indicator itself.

### Market Intelligence (INTEL_HEAD → 2 analysts)
TECHNICAL, SENTIMENT

**Why:** Cross-cutting functional analysis that domain analysts lack.
TECHNICAL reads charts and indicators across ALL sectors — catches
overbought/oversold conditions domain analysts miss because they're
focused on narrative. SENTIMENT monitors retail flows and insider
transactions — leading indicators for short squeezes and distribution.

---

## Open Questions (To Resolve During Implementation)

1. **Should structures know about each other?**
   No — each structure operates as if it's the only one.
   Agents in Structure A don't know B and C exist.

2. **Should the same daily data/prompt be used across structures?**
   Yes — identical data feed, identical overnight events.
   Only the organizational response differs.

3. **What happens when Structure C's optimizer and Structure A's consensus
   agree?** This is the highest-conviction signal — independent algorithmic
   synthesis AND peer debate reached the same conclusion. Track this as
   a separate "meta-signal" for research purposes.

4. **How do we handle the scale asymmetry in the comparison?**
   The Firm has 27 analysts vs the Council's 13. If the Firm outperforms,
   is it topology or scale? We acknowledge this as a feature: hierarchy's
   advantage IS scale. The question is whether that scale advantage
   justifies the management overhead (10 extra agents) and information
   filtering cost.
