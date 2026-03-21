# Multi-Agent Hedge Fund — Master Project Context

> This file is loaded into every Claude Code session automatically. It is the single source of truth for bootstrapping a fresh session with full project context. **Maintain this file as the project evolves.**

---

## Who Is Samuel

Samuel (邓景溪), 21, Chinese-Canadian CS student at USF (graduating December 2026). Founding engineer at Wondera ($50M valuation AI startup) — sole architect of the entire agent orchestration layer (AUTONOMOUS_EXECUTION.md, 5,335 lines). Expert in context engineering, multi-agent systems, and semiconductor supply chains.

Comp at Wondera: $6K/month part-time, $12K/month full-time (summer), 1% equity (negotiating to 3-4%).

---

## What This Project Is

A multi-agent financial intelligence system ("hedge fund simulation") that:
- Runs multiple AI analyst agents with persistent identities, each specializing in a domain (semiconductors, macro, China, oil, crypto, etc.)
- Tests multiple organizational topologies simultaneously against the same simulated portfolio
- Tracks predictions with timestamps for grading over 6+ months
- Focuses primarily on the semiconductor supply chain
- Uses Claude Opus 4.6 via Anthropic API directly (no OpenClaw)
- Paper trades with real market data; $100 real money as parallel learning experiment

**This is research, not a hack project.** Cross-domain validation of findings from Wondera. Potential publication: "Multi-agent organizational topology and its effect on decision quality in financial reasoning."

---

## Timeline

- March 2026: Architecture design + data pipeline (CURRENT)
- April 2026: Single-agent daily macro briefing working
- May 2026: Multi-agent system with all topologies running
- May-August 2026: Full-time at Wondera + system collecting data in background
- September 2026: 4+ months graded predictions, initial analysis
- December 2026: Graduate, publish findings, portfolio piece

---

## Key Research Findings from Wondera (The Foundation)

These are empirical, from running 100+ agents in production. They are the hypotheses being tested in the financial domain:

1. **Context engineering > prompt engineering.** What you put INTO the agent's context window matters more than how you instruct it.
2. **Asymmetric information (信息差) drives emergent social dynamics.** Agents with different information develop negotiation, deception, alliance-forming behaviors.
3. **Character-grounded stubbornness beats role-assigned disagreement.** Deep identity produces genuine sustained disagreement. "Disagree more" instructions produce fake pushback.
4. **Pacing matters enormously.** Slower pacing = better reasoning. Agents should read constantly but decide slowly.
5. **The $20K API lesson:** Never let leadership discover major expenses by looking at a dashboard. Always communicate cost trajectory upfront.

---

## Key Findings from the Collab Prototype Experiment

Source: `collab_prototype/` — Samuel's multi-agent music collaboration experiments at Wondera.

### Evolution (3 stages)
1. **Feb 21 (naive):** 2 original characters, task-oriented. Quick convergence by round 4-5. Lifeless.
2. **Feb 22 (breakthrough):** 5 original characters, 6 interaction modes. Organic song emergence in hangout mode. Convergence still kills by round 6.
3. **Mar 2-3 (revolution):** Pop-culture characters (Squidward, SpongeBob, etc.) with 6-layer identity stacks, 90 asymmetric relationship entries, private DMs, weighted random turns, phase-based energy nudges. Squidward held position for 64 rounds. Parallel sub-plots in DMs.

### What Killed Conformity (ranked by effectiveness)
1. **Private DMs** — single most effective anti-convergence mechanism. Hidden alliances, conditional cooperation, information asymmetry.
2. **Character-grounded stubbornness** — Squidward resists because he IS Squidward, not because a prompt says "disagree."
3. **Asymmetric relationships** — A→B ≠ B→A.
4. **Topic as suggestion, not assignment** — "The CIO just got off a call..." > "Analyze tariff impact."
5. **Weighted random turns** — Some agents speak rarely but devastatingly (risk manager pattern).

### What Failed
- "Disagree more" instructions → manufactured fake disagreement
- Contrarian agent role → gimmick
- Round-robin turn order → unnatural
- Tool-generated content → universally rejected (but useful as foil for critique)

---

## AgentOS Architecture (Ported from Wondera)

Distilled reference: `AGENT_ARCHITECTURE_REFERENCE.md`

### Three-Document Identity Model
| Document | Churn | Purpose |
|----------|-------|---------|
| `identity.md` | Slow | WHO the agent is — core personality, locked rules, evolvable convictions |
| `memory.md` | Moderate | WHAT the agent knows — relationships, goals, track record |
| `recent.md` | High | WHAT just happened — last session, open threads, next-wake tasks |

### Memory System (Three-Tier)
- **Hot** (identity.md + recent.md): always in context (~3-5K tokens)
- **Warm** (memory.md): pre-injected relevant blocks via keyword search (<1ms, no LLM call) + searchable via `recall`
- **Archive**: full history, keyword searchable (future: vector similarity)

### Key Patterns That Transfer Directly
- Viewport model (collapse data sources to one-liners when attention shifts)
- Engagement ledger (preserves full data outside context for audit trail)
- Action buffer / atomic commit (nothing publishes until end_cycle)
- Context processor registry (cheap LLM handles all memory writes)
- `[unresolved]` tag for persistent disagreements (protected from compression)
- Conviction pool of paired oppositions for anti-convergence
- TOON format (30-60% fewer tokens than JSON for tabular data)
- Research sub-agent pattern (read-only cheap model, returns <500 token summary)

---

## Real-World Org Structure Research

### Top-Down Firms → `docs/research/top-down-firm-structures.md`

Three archetypes:
1. **Centralized Intelligence** (Bridgewater): believability-weighted voting by track record per domain, radical transparency, codified investment logic as explicit decision rules
2. **Decentralized Pods** (Citadel/Millennium): autonomous PMs + central risk veto, dynamic capital allocation, fast failure (5-7% drawdown = cut)
3. **Unified System** (Renaissance): ALL signals feed ONE model, no star culture, flat collaboration, power in combining many weak signals

Universal patterns across ALL top firms:
- Separation of alpha generation and risk management (productive tension)
- Track-record-based influence weighting
- Deliberate information architecture (who sees what)
- Fast failure mechanisms
- Infrastructure as moat

### Peer-Level Structures → `docs/research/peer-level-investing-structures.md`

Key models: NAIC investment clubs, Tiger Cubs network, Good Judgment Project superforecaster teams, prediction markets.

Universal success factors:
- Analyze independently THEN share (prevents anchoring/groupthink)
- Standardized analytical framework per domain (common language)
- Transparent cumulative track records → earned meritocracy
- Independent convergence as high-conviction signal
- Structured dissent (mandatory bear case)
- Social process discipline (friction against panic selling, FOMO, confirmation bias)

---

## Analytical Foundation (Designed 2026-03-21)

This is the intellectual core of the system — how agents think and what they know.

### Three-Layer System Prompt Architecture
- **Layer 1 — Universal Foundation (~850 tokens, ALL agents):** Identity frame ("unaligned analyst, not an assistant"), 14 world mechanics axioms (compressed), 4 reasoning rules (compressed), analytical commitment rules, forbidden patterns, information hierarchy, cognitive checks
- **Layer 2 — Domain Framework (~600 tokens, per role):** Domain-specific causal chains, common misconceptions, key data sources, unique value to the team
- **Layer 3 — Identity (~800 tokens, per agent):** Conviction pool positions, personality quirks, asymmetric relationships, evolved traits (from identity.md)

### Four Reasoning Rules (the analytical DNA)
1. **Depth-First Analysis** — Ask "why one level deeper" until you hit structural bedrock (demographics, geography, resources, incentives). Most media stops at L1. Agents must reach L3-4.
2. **Bottleneck Descent** — When demand > supply at Layer N, investigate Layer N-1. Demand propagates down the supply chain with a time lag = your investment window. Check for monopoly concentration (80%+ share = highest conviction). Run parallel tracks across branches.
3. **Crack Detection** — Monitor Minsky stage (hedge → speculative → Ponzi). 7 measurable exit signals: revenue-investment divergence, insider selling, credit tightening, narrative degradation, supply chain reversal, greater fool dynamics, position crowding. When 3+ signals appear, begin systematic exit.
4. **Counterparty Test** — For every position, articulate who's on the other side and why they're wrong. If you can't make the opposing case as strongly as your own, you don't understand the trade.

### Neutrality Principle
Agents have no default loyalty (unaligned). They adopt a domain lens when assigned and commit fully to what that lens reveals. Objectivity emerges at the SYSTEM level from collision of multiple perspectived analyses. Character-grounded perspective > fake neutrality.

### Analytical Commitment
Every analysis must end with: (1) directional call with timeframe, (2) confidence level, (3) kill condition. Hedging, "both sides" framing, and vague monitoring are explicitly forbidden.

Full design: `docs/design/analytical-foundation.md`

### Organizational Structures (Designed 2026-03-21)

Three topologies running simultaneously. Same agents, same data, same knowledge. Only variable is topology. Design doc: `docs/design/organizational-structures.md`

**Structure A "The Council"** (11 agents) — Peer network. All analysts are equals. Full transparency + private DMs. Decisions by believability-weighted consensus + extremized mean. Risk agent has veto power. Inspired by Tiger Cubs, GJP superforecasters, NAIC clubs.

**Structure B "The Firm"** (15 agents) — 4-layer pyramid. L1: 10 domain analysts → L2: 3 sector heads (Tech & Supply Chain, Macro & Rates, Geopolitics & Resources) → L3: CIO → L4: Risk Committee (sees everything). Progressive information filtering. Info barriers between sectors at L1. CIO sees only sector synthesis, not raw data. Inspired by Point72, Citadel, Bridgewater.

**Structure C "The Model"** (10 agents) — No interaction. Agents produce independent structured signals. Deterministic optimizer (code, not LLM) combines signals via believability weighting + correlation management + risk constraints. No meetings, no DMs. The experimental control group. Inspired by Renaissance.

Estimated combined cost: $160-245/month.

### Data Pipeline (Designed 2026-03-21)

How agents get information. Design doc: `docs/design/data-pipeline.md`

- **RSS-first discovery** — YouTube channels, news outlets, Substacks, podcasts all via free RSS feeds. No API keys needed for discovery.
- **YouTube transcript pipeline** — RSS feed per subscribed channel → `youtube-transcript-api` for transcript → context processor summarizes to 200-500 tokens. Full transcript archived and recallable. ~$0.02/day for 50 channels.
- **Per-agent subscriptions** — Each agent subscribes to sources relevant to their domain (YouTube channels, RSS feeds, EDGAR tickers, FRED series). Stored in agent config.
- **Morning feed compiled overnight** — Agents wake to a compiled briefing with market data, new filings, YouTube summaries, news. No real-time interrupts except priority events.
- **Six source categories:** Market data (FRED, Yahoo Finance), News/filings (RSS, EDGAR), YouTube, Podcasts/Substacks, Earnings calls, Knowledge base (books, our docs)
- **Total ingestion cost: ~$3.60/month.** Agent reasoning (Opus 4.6) is the real cost at $50-100/month.

---

## Design Plan Status

Living document: `docs/design/DESIGN_PLAN.md`

```
[x] Area 1  — Org Structures (3 variants: Council, Firm, Model)
[x] Area 8  — Knowledge Base (14 axioms, 3 reference docs)
[x] Area 9  — Anti-Bias & Analytical Discipline (system prompt, conviction pool)
[~] Area 5  — Tool Schemas (data pipeline done, tool defs pending)
[ ] Area 2  — Agent Role Roster
[ ] Area 3  — Meeting Cadence & Communication
[ ] Area 4  — Runtime Loop & Context Engineering
[ ] Area 6  — Trade Execution & Portfolio Tracking
[ ] Area 7  — Monitoring Dashboard & Human Interface
```

---

## Working Preferences

- **Subagents must write MD files**, not just return text. Point to files, give verbal summary.
- **ASCII illustrations only.** No browser-based visuals. Context is expensive.
- **Persist confirmed decisions to docs incrementally.** Don't hold design only in conversation.
- **Meta-plan before thinking.** Structure the research/brainstorm approach before diving in.
- **Always communicate cost trajectory upfront.** Never surprise with a bill.
- **Controlled cadence over agent autonomy.** Never let agents decide when to meet.
- **This is research.** Timestamp everything. Document findings for publication.

---

## File Structure

```
multi-agent-hedge-fund/
├── CLAUDE.md                              ← YOU ARE HERE (master context)
├── AGENT_ARCHITECTURE_REFERENCE.md        ← Distilled AgentOS patterns for this project
├── collab_prototype/                      ← Wondera experiment (code + transcripts)
│   ├── collab.py                          ← 3,529 lines, core experiment logic
│   ├── MULTI_AGENT_EXPERIMENT_FINDINGS.md
│   └── transcripts/                       ← Full experiment transcripts
└── docs/
    ├── design/
    │   ├── DESIGN_PLAN.md                 ← Living design tracker with confirmed decisions
    │   ├── analytical-foundation.md       ← System prompt architecture (Layer 1/2/3 design)
    │   ├── data-pipeline.md              ← How agents get info (YouTube, RSS, EDGAR, FRED)
    │   └── organizational-structures.md  ← 3 topologies: Council, Firm, Model
    ├── knowledge/
    │   ├── world-mechanics.md             ← 14 axioms with full explanations + sources
    │   ├── reasoning-examples.md          ← Worked examples (AI supply chain, Mao, Iran, EV, defense)
    │   └── exit-signals.md                ← Minsky cycle, 7 crack signals, exit protocol
    └── research/
        ├── peer-level-investing-structures.md  ← NAIC, Tiger Cubs, GJP, prediction markets
        ├── top-down-firm-structures.md         ← Bridgewater, Renaissance, Citadel, DE Shaw, Point72
        └── youtube-ingestion-pipeline.md       ← Technical research on YT API, RSS, transcript extraction
```

---

## How to Maintain This File

When significant new context is established in a session:
1. Update the relevant section of this file (or add a new section)
2. If the context is too detailed for this file, write it to a doc and add a pointer here
3. Keep this file under ~500 lines — it's a bootstrap, not an encyclopedia
4. Confirmed design decisions should be appended to the "Design Plan Status" section with one-line summaries
5. New research findings should get their own doc in `docs/research/` with a pointer added here
