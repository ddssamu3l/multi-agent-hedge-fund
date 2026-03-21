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

| Agent | Domain | Role |
|-------|--------|------|
| MACRO | Monetary policy, rates, CPI/jobs, Fed/ECB/BoJ | Domain analyst |
| SEMI | Semiconductor supply chain, chip demand/supply, packaging | Domain analyst |
| CHINA | Chinese economy, 土地财政, PBoC, trade, dual circulation | Domain analyst |
| OIL | Energy, petrochemicals, OPEC, shipping, renewables transition | Domain analyst |
| CRYPTO | Digital assets, DeFi, on-chain data, regulatory landscape | Domain analyst |
| TECH | SaaS, cloud, AI companies, enterprise software, consumer tech | Domain analyst |
| LIQUIDITY | Credit conditions, yield curves, bank lending, shadow banking | Domain analyst |
| JAPAN | BoJ, yen carry trade, Japanese equities, demographics | Domain analyst |
| GEOPOLITICS | US foreign policy, alliances, conflicts, sanctions, NATO | Domain analyst |
| MINERALS | Copper, lithium, rare earths, mining, commodity super-cycles | Domain analyst |
| RISK | Cross-cutting: correlation, concentration, drawdown, Minsky stage | Referee (veto power) |

**Total: 11 agents** (10 domain + 1 risk referee)

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

**Layer 1 — Domain Analysts (ground floor)**

| Agent | Domain | Reports To |
|-------|--------|-----------|
| SEMI | Semiconductor supply chain | Tech & Supply Chain Head |
| TECH | SaaS, cloud, AI companies | Tech & Supply Chain Head |
| CRYPTO | Digital assets, DeFi, on-chain | Tech & Supply Chain Head |
| MACRO | Monetary policy, rates, CPI/jobs | Macro & Rates Head |
| LIQUIDITY | Credit conditions, yield curves | Macro & Rates Head |
| JAPAN | BoJ, yen carry trade, demographics | Macro & Rates Head |
| CHINA | Chinese economy, PBoC, trade | Geopolitics & Resources Head |
| OIL | Energy, petrochemicals, OPEC | Geopolitics & Resources Head |
| GEOPOLITICS | US foreign policy, conflicts, sanctions | Geopolitics & Resources Head |
| MINERALS | Copper, lithium, rare earths, mining | Geopolitics & Resources Head |

**Layer 2 — Sector Heads (middle management)**

| Agent | Sector | Synthesizes | Reports To |
|-------|--------|-------------|-----------|
| TECH_HEAD | Tech & Supply Chain | SEMI + TECH + CRYPTO | CIO |
| MACRO_HEAD | Macro & Rates | MACRO + LIQUIDITY + JAPAN | CIO |
| GEO_HEAD | Geopolitics & Resources | CHINA + OIL + GEOPOLITICS + MINERALS | CIO |

**Layer 3 — CIO (the strategist)**

| Agent | Role |
|-------|------|
| CIO | Synthesizes sector heads. Makes final allocation decisions. Runs Idea Dinners. |

**Layer 4 — Risk Committee (the check)**

| Agent | Role |
|-------|------|
| RISK_COMMITTEE | Sees ALL layers unfiltered. Validates CIO decisions. Can block with stated reason. |

**Total: 15 agents** (10 domain + 3 sector heads + 1 CIO + 1 risk committee)

### Layer Responsibilities

**Layer 1 analysts:**
- Do the actual research: read filings, pull data, watch YouTube
  transcripts, analyze earnings calls, form domain-specific views
- Produce structured reports for their sector head
- Have deep but narrow vision — they know their domain cold but
  don't see the full portfolio picture
- Can DM other L1 analysts WITHIN their sector
- Cannot DM analysts in other sectors (information barrier, Citadel model)

**Layer 2 sector heads:**
- Synthesize across their domain analysts
- See patterns analysts can't see individually. Example: Tech Head
  sees that SEMI's HBM shortage + TECH's cloud capex surge + CRYPTO's
  mining demand all point to the same memory bottleneck
- FILTER: decide what's signal vs noise for the CIO. Not everything
  goes up. Sector heads own this judgment call.
- Push BACK to analysts: "your thesis contradicts SEMI's data, reconcile"
- Produce sector-level synthesis memos for the CIO
- Can DM other sector heads (cross-sector intelligence sharing)

**Layer 3 CIO:**
- Only sees sector-level synthesis, NOT raw analyst output
- Makes cross-sector connections: Tech Head says "GPU demand insane" +
  Macro Head says "rates about to rise" = CIO sees the collision
- Runs Idea Dinners with sector heads: structured presentations +
  interrogation ("tell me why I'm wrong" — Bridgewater style)
- Makes final allocation decisions with stated reasoning
- Can pull raw analyst data via request (asks sector head to surface it)
  but doesn't browse it by default

**Layer 4 Risk Committee:**
- Sees ALL layers: raw analyst data + sector synthesis + CIO decisions
- The ONLY entity with the complete unfiltered picture
- Can block CIO decisions with stated reasoning
- Monitors: concentration, correlation, Minsky stage, crack signals,
  drawdown limits, sector exposure
- CIO can override risk block with supermajority (>70%) sector head support
  and stated reasoning (creates paper trail)

### Information Architecture
```
                    WHAT EACH LAYER SEES
   ═══════════════════════════════════════════════

   Layer 1 analysts:
     ✓ Own domain data (full depth)
     ✓ Other analysts in same sector (DMs)
     ✗ Other sectors' analysts
     ✗ Sector head synthesis
     ✗ CIO decisions (until communicated down)

   Layer 2 sector heads:
     ✓ Their analysts' full output
     ✓ Other sector heads (DMs)
     ✓ CIO questions and requests
     ✗ Other sectors' raw analyst data
     ✗ CIO's final reasoning (until communicated)

   Layer 3 CIO:
     ✓ All sector head synthesis memos
     ✓ Historical allocation decisions + outcomes
     ✗ Raw analyst data (unless requested)
     ✗ Analyst DMs

   Layer 4 Risk Committee:
     ✓ EVERYTHING (all layers, all DMs, all data)
```

### Decision Flow
1. **Analysts research daily** → produce domain reports
2. **Sector heads synthesize weekly** → produce sector memos
3. **CIO runs Idea Dinner monthly** → sector heads present, CIO
   interrogates, CIO drafts allocation memo
4. **Risk Committee reviews** → approves, blocks, or requests changes
5. **CIO finalizes** → allocation decision executed

### Meeting Format
- **Daily within-sector standups** (~15 turns):
  L1 analysts brief their sector head. Quick updates, flags,
  open questions. Sector head assigns research tasks.
- **Weekly sector synthesis meeting** (~25 turns):
  Sector head presents draft synthesis to their analysts.
  Analysts challenge, add context, push back. Sector head
  finalizes memo for CIO.
- **Monthly Idea Dinner** (~40 turns):
  CIO + all 3 sector heads. Each sector head presents their
  strongest conviction and biggest concern. CIO interrogates.
  Open cross-sector debate. CIO drafts allocation.
- **Monthly Risk Review** (~20 turns):
  Risk Committee reviews CIO's allocation against all raw data.
  Approves, blocks, or flags specific concerns.

### What This Tests
- Does progressive filtering remove noise or lose signal?
- Does the CIO miss cross-domain connections that only exist
  at the ground floor (the Ajinomoto problem)?
- Does the sector head layer add value by synthesizing, or does
  it just add latency and information loss?
- Does the Risk Committee's full visibility compensate for the
  CIO's filtered view?
- How does information flow efficiency compare to peer debate?

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

| Agent | Domain | Role |
|-------|--------|------|
| MACRO | Monetary policy, rates, CPI/jobs | Independent signal generator |
| SEMI | Semiconductor supply chain | Independent signal generator |
| CHINA | Chinese economy, PBoC, trade | Independent signal generator |
| OIL | Energy, petrochemicals, OPEC | Independent signal generator |
| CRYPTO | Digital assets, DeFi, on-chain | Independent signal generator |
| TECH | SaaS, cloud, AI companies | Independent signal generator |
| LIQUIDITY | Credit conditions, yield curves | Independent signal generator |
| JAPAN | BoJ, yen carry trade | Independent signal generator |
| GEOPOLITICS | US foreign policy, conflicts | Independent signal generator |
| MINERALS | Copper, lithium, rare earths | Independent signal generator |

**Total: 10 agents** + portfolio optimizer (code, not agent)

No risk agent. Risk is embedded as constraints in the optimizer.

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

| Structure | Agents | Daily Meeting Tokens | Monthly Decision Tokens | Data Ingestion | Total |
|-----------|--------|---------------------|------------------------|----------------|-------|
| A (Council) | 11 | ~$40-60 (1 daily round-table) | ~$20-30 | ~$4 | ~$65-95 |
| B (Firm) | 15 | ~$50-80 (sector standups + weekly synthesis) | ~$30-50 (Idea Dinner + risk review) | ~$4 | ~$85-135 |
| C (Model) | 10 | $0 (no meetings) | ~$5-10 (signal generation only) | ~$4 | ~$10-15 |
| **Combined** | | | | | **~$160-245/month** |

Structure C is dramatically cheaper because agents never talk to each
other. This is itself an interesting finding if C outperforms A and B.

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

### Tech & Supply Chain
- **SEMI** — Semiconductor supply chain, chip fabrication, packaging, equipment
- **TECH** — SaaS, cloud, AI companies, enterprise software, consumer tech
- **CRYPTO** — Digital assets, DeFi, on-chain analytics, regulatory landscape

**Why this grouping:** These three domains share supply chain dependencies.
AI demand drives GPU demand drives chip fabrication drives memory drives
packaging. Crypto mining adds GPU demand. Cloud capex drives all of it.
The sector head can see the full demand-supply picture.

### Macro & Rates
- **MACRO** — Fed/ECB/BoJ policy, CPI, employment, GDP, fiscal policy
- **LIQUIDITY** — Credit conditions, yield curves, bank lending, shadow banking, credit spreads
- **JAPAN** — BoJ yield curve control, yen carry trade, Japanese equities, demographics

**Why this grouping:** Monetary policy, credit conditions, and the yen
carry trade are deeply interlinked. BoJ moves affect global liquidity.
Credit conditions determine which sectors can access capital. The sector
head can see the full monetary transmission picture.

### Geopolitics & Resources
- **CHINA** — Chinese economy, PBoC, 土地财政, trade policy, dual circulation, Hong Kong
- **OIL** — Energy, petrochemicals, OPEC+, shipping lanes, renewable transition
- **GEOPOLITICS** — US foreign policy, alliances, conflicts, sanctions, military
- **MINERALS** — Copper, lithium, rare earths, mining, commodity super-cycles

**Why this grouping:** Geopolitics drives resource flows. China's economy
drives commodity demand. US sanctions reshape supply chains. Oil is the
currency of geopolitical power. The sector head can see the full
geopolitical-resource nexus.

---

## Open Questions (To Resolve During Implementation)

1. **Should Structure A agents know that Structures B and C exist?**
   Probably not — it could bias their behavior. Each structure should
   operate as if it's the only one.

2. **Should the same random seed / conversation starter be used across
   structures for fair comparison?** Yes — the daily topic/prompt should
   be identical. Only the organizational response differs.

3. **How do we handle Structure B's staggered meetings?** L1 standups
   happen first, then L2 synthesis, then L3 Idea Dinner. This means
   Structure B's final decision incorporates more processing time.
   Is this an advantage (more thought) or disadvantage (slower reaction)?

4. **Can Structure B's CIO request raw analyst data?** Current design
   says yes, via sector head. This is a pressure release valve — if the
   CIO suspects the sector head is filtering too aggressively, they can
   drill down. But it undermines the pure hierarchy experiment.

5. **What happens when Structure C's optimizer and Structure A's consensus
   agree?** This is the highest-conviction signal — independent algorithmic
   synthesis AND peer debate reached the same conclusion. Should we track
   this as a separate "meta-signal"?
