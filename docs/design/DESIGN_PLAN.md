# Hedge Fund System Design Plan

> Living document. Updated as we work through each area.

## Status Key
- [ ] Not started
- [~] In progress
- [x] Complete

---

## Design Areas (in dependency order)

### [x] Area 1: Organizational Structures (3 variants) — COMPLETE
- [x] Research: peer-level clubs/networks → `docs/research/peer-level-investing-structures.md`
- [x] Research: top-down hedge fund firms → `docs/research/top-down-firm-structures.md`
- [x] Decision: 3 structures — Council (peer), Firm (4-layer pyramid), Model (unified synthesis)
- [x] Output: `docs/design/organizational-structures.md`

### [~] Area 2: Agent Role Roster
- Depends on: Area 1
- [x] Reference: Wondera agent creation form → `docs/reference/WONDERA_AGENT_CREATION_FORM.md`
- [x] Design: 11-section identity schema from scratch → `docs/design/agent-identity-schema.md`
- [x] Design: relationship schema (asymmetric, with tags and friction)
- [ ] Output: filled identity.md for each of the 10 domain agents (future session, one by one)
- [ ] Output: filled relationships for all agent pairs (future session)

### [x] Area 3+4: Runtime Cadence & Meeting Protocol (merged) — COMPLETE
- Depends on: Area 1 + 2
- Ports from: collab_prototype (hangout mode, DMs, weighted turns) + AgentOS (viewport, memory tiers, wake/sleep)
- [x] Research: meeting frameworks & anti-groupthink mechanisms → `docs/research/meeting-frameworks.md`
- [x] Research: analyst journal & decision-tracking frameworks → `docs/research/analyst-journal-frameworks.md`
- [x] Decision: daily lifecycle — 5pm ET wake, morning meeting, execution block, evening meeting, 10:30pm sleep
- [x] Decision: write-first principle — all agents submit structured briefs before any meeting discussion
- [x] Decision: structured decision objects — all recommendations, predictions, theses are parseable schemas, not prose
- [x] Decision: personal track record (mock trades, no portfolio weight) vs org track record (real portfolio decisions)
- [x] Decision: trade execution — conditional limit orders with kill prices, pre-execution check at next market open
- [x] Decision: track record lifecycle — code creates entries, code monitors, agent reflects, context processor audits, code aggregates
- [x] Design: pre-meeting brief schema (5 sections, structured recommendations) → `docs/design/runtime-documents.md`
- [x] Design: personal journal schema (8 sections: daily log, theses, watchlist, triggers, predictions, errors, track record, self-assessment) → `docs/design/runtime-documents.md`
- [x] Design: org log schema (daily record, portfolio, strategy, collective predictions, decision log, believability scores, org track record) → `docs/design/runtime-documents.md`
- [x] Design: structured decision object lifecycle (creation → monitoring → validation → aggregation → pattern detection → identity evolution) → `docs/design/runtime-documents.md`
- [x] Design: per-structure meeting protocols → `docs/design/meeting-protocols.md`
- [x] Design: DM protocol (request/accept, visibility rules, nemawashi pattern) → `docs/design/meeting-protocols.md`
- [x] Design: turn management (weighted random + callout priority, juniors-first for Firm, end-of-meeting check-in) → `docs/design/meeting-protocols.md`
- [x] Design: decision mechanics — Council (0.60 weighted vote), Firm (CIO final say), Model (optimizer)

### [x] Area 5: Tool Schemas — COMPLETE
- Depends on: Area 2 + 4
- [x] Research: what data APIs exist (market, news, filings) → `docs/research/youtube-ingestion-pipeline.md`
- [x] Design: data pipeline architecture → `docs/design/data-pipeline.md`
- [x] Decision: per-agent tool access by structure (Council: all, Firm: sector-scoped read + cross-sector DM, Model: no communication tools)
- [x] Decision: always-loaded vs searchable tool split (8 always-loaded, 7+ searchable on demand)
- [x] Decision: TradingAgents integrated as research sub-agent tool (no authority, no gating, free use)
- [x] Output: tool schema definitions → `docs/design/tool-schemas.md`

### [x] Area 6: Trade Execution & Portfolio Tracking — COMPLETE
- [x] Decision: conditional limit orders with kill prices, pre-execution check at market open
- [x] Decision: personal mock trades vs org real trades separation
- [x] Decision: paper portfolio — track entry/exit prices and return %, no real broker, no real capital
- [x] Decision: proposer's size stands — agent sets size_pct, group votes on thesis not sizing
- [x] Decision: auto-rebalance — trim all positions proportionally to fund new approved trades
- [x] Decision: position lifecycle (PROPOSED → APPROVED → EXECUTED → OPEN → CLOSED)
- [x] Decision: 6 exit triggers (stop price, target price, kill condition, agent exit, time expiry, auto-rebalance)
- [x] Decision: pre-execution 5% sanity check — hold order if price moved >5% overnight
- [x] Decision: SQLite for MVP (positions, portfolio snapshots, orders, rebalance log)
- [x] Decision: $100 real money is manual mirror, not system-managed
- [x] Output: trading system spec → `docs/design/trade-execution.md`

### [~] Area 7: Monitoring Dashboard & Human Interface
- Depends on: Area 6
- [x] Decision: Web app (Next.js frontend + Python FastAPI backend)
- [x] Decision: GUI for org configuration — users add/remove agents, create sectors, edit CIO prompts via UI (no YAML editing)
- [x] Decision: topology as configuration, not code — engine is topology-agnostic, Structure interface drives meeting/decision/visibility logic
- [x] Decision: open-source two models — Committee (flat, add agents freely) and Firm (3-level hierarchy, configurable sectors)
- [x] Decision: engine-first — run headless for months collecting data, add GUI when there's something to visualize
- [ ] Design: GUI wireframes and component spec
- [ ] Design: API contract between frontend and backend
- [ ] Output: dashboard implementation

### [x] Area 8: Financial Knowledge Base — COMPLETE
- [x] Research: deep macro knowledge frameworks
- [x] Decision: what bakes into identity vs. retrieved at runtime
- [x] Output: knowledge architecture + key frameworks doc
- Design doc: `docs/design/analytical-foundation.md`
- Reference docs:
  - `docs/knowledge/world-mechanics.md` (14 axioms, full explanations)
  - `docs/knowledge/reasoning-examples.md` (worked examples for all 4 reasoning rules)
  - `docs/knowledge/exit-signals.md` (Minsky cycle, 7 crack signals, exit protocol)

### [x] Area 9: Anti-Bias & Analytical Discipline — COMPLETE
- [x] Decision: "no unexamined bias" — agents have no default loyalty but adopt domain lens fully
- [x] Output: system prompt Layer 1 (universal foundation, ~850 tokens)
- [x] Output: analytical commitment rules + forbidden patterns
- [x] Output: financial conviction pool (7 axes)
- Ports from: collab_prototype anti-convergence (forbidden phrases, conviction pool)
- Design doc: `docs/design/analytical-foundation.md`

---

## Dependency Graph

```
Area 8 (Knowledge) ──────────────────────────┐
                                              ▼
Area 1 (Org Structures) ──► Area 2 (Roster) ──► Area 3+4 (Runtime & Meetings) ──► Area 5 (Tools)
                                              │           │
                                              ▼           ├──► Area 6 (Trading, partial)
                                         Area 9 (Anti-Bias)
                                                          │
Area 6 (Trading) ──► Area 7 (Dashboard)    ← independent track (6 partially done via 3+4)
```

## Confirmed Decisions Log

> Append here as we lock things in during brainstorming.

### 2026-03-21: Knowledge & Analytical Foundation
1. **Three-layer system prompt architecture:** Layer 1 (universal analytical foundation, ~850 tokens, all agents) → Layer 2 (domain framework, ~600 tokens, per role) → Layer 3 (identity from identity.md, per agent)
2. **14 world mechanics axioms** hardcoded as compressed one-liners in system prompt; full explanations in `docs/knowledge/world-mechanics.md` recalled on demand via keyword pre-injection
3. **4 reasoning rules:** Depth-First Analysis, Bottleneck Descent, Crack Detection, Counterparty Test — compressed in system prompt, full worked examples in `docs/knowledge/reasoning-examples.md`
4. **Analytical Commitment rules:** agents MUST take definitive stances with directional call + confidence + kill condition. Forbidden: hedging, "both sides" framing, vague monitoring, equal-probability scenarios
5. **Neutrality principle resolved:** agents have no default loyalty (unaligned) but adopt domain lens fully when assigned. Objectivity emerges at SYSTEM level from collision of perspectived analyses, not from individual agent neutrality
6. **Information hierarchy:** Tier 1 (raw data/filings) > Tier 2 (earnings calls/academic) > Tier 3 (financial media) > Tier 4 (mainstream news/social media). All media carries selection bias.
7. **Token budget:** ~5600 tokens fixed overhead per agent call (Layer 1-4). Acceptable for monthly decision cadence with daily meetings.
8. **Reference docs recalled on demand** via programmatic keyword pre-injection (<1ms, no LLM call): world-mechanics, reasoning-examples, exit-signals
9. **Data pipeline: RSS-first discovery** — YouTube channels, news, blogs all via free RSS. Transcripts via youtube-transcript-api. Context processor (cheap LLM) summarizes raw data into 200-500 token briefings. Full text in archive, recallable on demand. Per-agent subscriptions in config. Morning feed compiled overnight. Total ingestion cost: ~$3.60/month.

### 2026-03-21: Organizational Structures
10. **Three structures running simultaneously:** Council (A, peer network, 11 agents), Firm (B, 4-layer pyramid, 15 agents), Model (C, unified synthesis, 10 agents + code optimizer)
11. **Council (A):** Tiger Cubs + GJP superforecaster inspired. Full transparency, DMs allowed, weighted consensus with risk veto. Believability-weighted aggregation + extremized mean.
12. **Firm (B):** Point72 + Citadel + Bridgewater inspired. 4 layers: L1 domain analysts → L2 sector heads (Tech&Supply, Macro&Rates, Geo&Resources) → L3 CIO → L4 Risk Committee. Progressive filtering. Information barriers between sectors at L1. Sector heads can DM cross-sector. Risk Committee sees everything.
13. **Model (C):** Renaissance inspired. No meetings, no interaction. Agents produce independent structured signals. Deterministic optimizer combines via believability-weighted aggregation + correlation management + risk constraints. Control group for social dynamics experiment.
14. **Same agents, same data, same knowledge across all three.** Only variable is organizational topology. Comparison framework: total return, Sharpe, max drawdown, prediction accuracy, cross-domain connection rate, information loss metric.
15. **Estimated combined cost: $160-245/month** (A: $65-95, B: $85-135, C: $10-15)
16. **Agent identity schema: 11 sections from scratch** (not ported from Wondera). Designed around analyst behaviors: research, form thesis, present, defend, challenge, be wrong, evolve. Key new fields: Analytical Method (blind spots, evidence threshold, primary lens), Under Pressure (challenged/wrong/consensus), Bottom Lines (hills + dig-in triggers + walk-away). Individual agent identities to be crafted in future sessions.

### 2026-03-21: Runtime Cadence & Meeting Frameworks
17. **Daily lifecycle: 5pm-10:30pm ET.** All agents wake simultaneously at 5pm ET (after US market close, most complete global data). Solo pre-prep → morning meeting → execution block → evening meeting → wind-down → sleep. Orders queue overnight, pre-execution check at 6am ET before market open.
18. **Write-first principle is non-negotiable.** Every meeting starts with all agents independently submitting structured pre-meeting briefs before any agent sees another's output. Validated by Delphi, GJP, NGT, Amazon memos, Point72 pitches — the single most consistent anti-groupthink finding across all research.
19. **Authentic dissent > assigned disagreement.** Nemeth's research confirms collab prototype finding: devil's advocacy is counterproductive. Only genuine, identity-grounded disagreement works. Never assign contrarian roles — disagreement must emerge from conviction pool conflicts.
20. **Structured decision objects, not prose.** All recommendations, predictions, and theses are explicit schemas with fixed fields (action, asset, confidence, kill_condition, counterparty, etc.) — parseable by code. System automatically creates track record entries from these structured outputs. Agents cannot choose what gets tracked.
21. **Personal track record ≠ org decisions.** Agent recommendations are mock trades in a personal paper portfolio — tracked against real market data for self-calibration and believability scoring, but carrying zero portfolio weight. Only calls that pass group decision process (vote/CIO/optimizer) become org decisions that move the actual portfolio. Rejected calls that turn out correct are the system's most valuable signal.
22. **Track record lifecycle: code + LLM hybrid.** Code creates entries (from structured outputs), code monitors conditions (market data, thresholds, calendar), main agent reflects on resolved entries (the learning mechanism), context processor audits reflection honesty, code aggregates stats, context processor detects systemic patterns quarterly, context processor promotes significant lessons to identity.md.
23. **Trade execution: conditional orders with pre-execution check.** Agents decide during their cycle, but all orders queue as conditional limit orders with max acceptable price and kill price. Pre-execution check at 6am ET verifies pre-market prices against limits before market open execution. No blind market orders ever.
24. **Five agent files per agent:** identity.md (slow churn), memory.md (moderate), recent.md (high), journal.md (daily operational), track_record.md (append-only, separate file). Org level: org_log.md + org_track_record.md per structure.
25. **Meeting research findings.** Key frameworks adopted: Delphi (anonymous iterative rounds), GJP (private forecast first, extremized mean), Amazon (juniors speak first, 6-page memo), Point72 (variant view requirement), McKinsey (obligation to dissent), nemawashi (pre-meeting DM consensus building), Intel (disagree and commit — separate debate from execution), Klein premortem (imagine failure, extract kill conditions). Full research: `docs/research/meeting-frameworks.md`
26. **Analyst journal frameworks.** Personal journals modeled on: Soros (reflexivity diary, scenario thinking), Druckenmiller (conviction sizing, streak tracking), Dalio (error log, decision criteria journal), Burry (primary source immersion), GJP (prediction records with Brier scoring), military DSM (if-then triggers), three-tier watchlists. Full research: `docs/research/analyst-journal-frameworks.md`

### 2026-03-23: Meeting Protocols & Decision Mechanics
27. **Morning meeting = info sharing + strategy equally.** Not just "what happened" but "what should we do about it." Convergence signals get recognized and acted on immediately.
28. **Evening meeting = full debate + vote.** DMs and private meetings during the day are preparation. Decisions involve the whole group — democratic (Council) and authoritarian (Firm) alike.
29. **100 turn limit per meeting, natural exit.** No fixed turn budget. Meetings run as long as needed up to 100 turns. Agents can leave when done. System sends reminders at 50/25/10 remaining.
30. **System code moderates.** No LLM moderator. Deterministic phase transitions, speaker selection, reminders, vote collection.
31. **Weighted random speaking + callout priority + end-of-meeting check-in.** Round-robin rejected (failed in collab prototype). Random prevents anchoring, callouts ensure relevant agents respond when referenced, check-in guarantees quiet agents aren't silenced.
32. **DMs (1-on-1) + private meetings (2+), request/accept, Risk sees all.** Two communication tools. Invitees can decline — prevents context blowup. Risk has full visibility in Structures A and B.
33. **Council decision: 0.60 weighted vote.** Believability-weighted. Risk veto is final — no override mechanism in a peer network.
34. **Firm decision: CIO has final say.** CIO sits in all meetings silently, only speaks to approve/reject recs in evening. CIO CAN override Risk veto (goes on permanent record, graded in track record). Analysts speak first (Amazon juniors-first).
35. **Firm: Idea Dinner removed.** CIO is present daily — no need for monthly pitch events. Daily standups + evening synthesis is sufficient.
36. **Firm: L1 info barriers are about default context, not communication bans.** Analysts don't see other sectors' published work automatically, but CAN DM across sectors. Evening meeting breaks barriers — all layers present, cross-sector discussion open.
37. **Model: no meetings, pure signal submission.** Agents produce structured signals, optimizer combines algorithmically. Risk is hard-coded constraints, not overridable. The experimental control group.
38. **Estimated total cost: $180-300/month** for all three structures combined (meetings + DMs + research + data ingestion).

### 2026-03-30: Tool Schemas
39. **TradingAgents (TauricResearch) integrated as research sub-agent tool.** Agents can invoke the TradingAgents multi-analyst pipeline on any ticker for a sanity check. Returns 5-tier rating + analyst reports + bull/bear debate + risk assessment. No authority, no gating, no voting weight — just a research instrument. ~$0.05-0.15 per call.
40. **Always-loaded vs searchable tool split.** 8 always-loaded tools (market data, read/write analysis, save/recall memory, submit rec/prediction, end_cycle). 7+ searchable tools discovered on demand via meta-tool (TradingAgents, FRED, earnings, web search, DM, meeting). Saves ~200-500 tokens per tool definition not loaded.
41. **Tool access varies by structure.** Council: all tools available to all agents. Firm: communication tools available but read_analysis scoped to sector + evening meeting. Model: no communication tools (DM, meeting), signal submission only.
42. **Verification sub-agent system.** 10 cheap LLMs (one per analyst), run daily before wake. Check all watchlist items, triggers, kill conditions. Leave notification summary + full status dashboard. ~$3-15/month.
43. **SHORT added to action enum.** All recommendation schemas now support SHORT with required additional fields: max_loss_pct, catalyst, squeeze_risk, minsky_stage. Short-specific risks documented in exit-signals.md.
44. **WATCH auto-hookup.** WATCH recommendations automatically create watchlist entries (Tier 2 if entry trigger present, Tier 3 otherwise). reasoning_snapshot field preserves full analytical context for future reference.
45. **Partial trigger mechanic.** Compound triggers track individual conditions as NOT_MET/APPROACHING/MET. Partial triggers surface in daily notifications so agents can monitor progression.

### 2026-03-31: Firm Expansion & Architecture
46. **Firm expanded to ~31 agents.** 23 L1 analysts across 7 sectors + 7 sector heads + CIO + Risk. Modeled on real fund sector coverage (Point72, Citadel, Balyasny). Sectors: Semiconductors (4 analysts), Technology (3), Macro & Rates (4, including dedicated JAPAN), Energy & Power (3), Geopolitics & Trade (4), Commodities (3), Crypto (2). Council stays at 11 (peer network limit). Model stays at 10. Hierarchy's advantage is scale — more specialists going deeper.
47. **Firm daily flow: strategy flows down, research flows up.** CIO sets strategic priorities in evening synthesis → sector heads relay as directives in morning standup → analysts execute assigned tasks first, personal research second → findings flow up through sector heads → CIO decides + sets new priorities. Cycle repeats daily.
48. **L1 analysts never attend evening synthesis.** Only sector heads + CIO + Risk (9 agents). Analysts learn about CIO decisions the next morning, filtered through their sector head's interpretation. One-day information delay. Genuine hierarchy.
49. **Sector heads are pure managers.** No independent research. They synthesize, filter, assign tasks, present upward, relay decisions downward. They choose what goes up to the CIO (signal vs noise filtering) and how to frame CIO decisions for analysts (downward filtering). Both filtering directions are part of the hierarchy experiment.
50. **Gold/silver: split responsibility.** METALS analyst (Commodities sector) covers supply/mining/demand. CREDIT analyst (Macro sector) monitors gold as a monetary/liquidity indicator. Cross-domain connection happens through DMs — not pre-assigned.
51. **Topology as configuration, not code.** Engine is topology-agnostic. Structure interface (get_morning_meetings, get_decision_mechanic, can_see, can_dm) drives all meeting/decision/visibility logic. Adding an agent = config entry + identity files. Adding a sector = config block. Switching topology = swap config. Code never changes.
52. **Open-source two models: Committee and Firm.** Committee: flat, add agents freely, they all participate as peers. Firm: 3-level hierarchy, configurable sectors, editable CIO system prompt and skills, add/remove agents per team via GUI.
53. **Web app: Next.js frontend + Python FastAPI backend.** GUI for org configuration (add/remove agents, create sectors, edit prompts). Engine runs headless first — GUI comes after data collection validates the experiment. Estimated combined cost: $225-330/month for all three structures.

### 2026-03-31: Trade Execution
54. **Paper portfolio, real prices.** No broker, no real capital. Track entry price, exit price, return %, holding period against real market data. Starting capital $1M theoretical for return calculation.
55. **Proposer's size stands.** Agent sets size_pct based on conviction. Group votes on thesis, not sizing. Sizing skill is part of what gets tracked in the track record.
56. **Auto-rebalance.** When new position approved and insufficient cash, system trims ALL existing positions proportionally. Positions trimmed below 1% get fully closed (dust cleanup). Pure math, no agent decision needed. Logged for audit trail.
57. **6 exit triggers.** Stop price hit (auto), target price hit (auto), kill condition met (flag for review), agent proposes exit (normal approval process), time expiry (flag for reassessment), auto-rebalance (system trim).
58. **Pre-execution 5% sanity check.** If overnight price moves >5% from approval price, order is HELD for re-evaluation. If price exceeds kill price, order is CANCELLED. Prevents stale-thesis entries after overnight gaps.
59. **Morning brief has no recommendations.** Agents lack cross-domain context at morning. Evening brief has recs informed by full day's shared context, DMs, and synthesis. Recs are now better quality because agents absorbed everyone else's morning findings before proposing trades.
60. **READY_TO_CLOSE meeting mechanic.** Agents signal when they're done discussing. >50% signals + one round since last REC-* → system transitions to vote. Natural meeting endings. 100 turn hard cap as safety net.
61. **SQLite for MVP.** Four tables: positions, portfolio_snapshots, orders, rebalances. Sufficient for tracking 3 structures × ~5-15 positions each over 6+ months.
