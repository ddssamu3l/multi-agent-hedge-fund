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

### [ ] Area 2: Agent Role Roster
- Depends on: Area 1
- [ ] Research: what domains actually move markets
- [ ] Decision: how many agents, what domains, what personality sketches
- [ ] Output: agent roster with identity drafts

### [ ] Area 3: Meeting Cadence & Communication
- Depends on: Area 1 + 2
- Ports from: collab_prototype (hangout mode, DMs, weighted turns)
- [ ] Decision: scheduled vs spontaneous meetings, accept/reject logic
- [ ] Output: meeting schedule + communication protocol

### [ ] Area 4: Runtime Loop & Context Engineering
- Depends on: Area 3
- Ports from: AgentOS (viewport, memory tiers, wake/sleep)
- [ ] Decision: how agents leave/rejoin conversations, turn weights
- [ ] Output: full agentic run loop specification

### [~] Area 5: Tool Schemas
- Depends on: Area 2 + 4
- [x] Research: what data APIs exist (market, news, filings) → `docs/research/youtube-ingestion-pipeline.md`
- [x] Design: data pipeline architecture → `docs/design/data-pipeline.md`
- [ ] Decision: per-agent tool access, shared vs exclusive tools
- [ ] Output: tool schema definitions

### [ ] Area 6: Trade Execution & Portfolio Tracking
- Mostly independent
- [ ] Decision: paper trading mechanics, position tracking, P&L calc
- [ ] Output: trading system spec

### [ ] Area 7: Monitoring Dashboard & Human Interface
- Depends on: Area 6
- [ ] Decision: Slack vs custom UI, how human injects events
- [ ] Output: dashboard spec + human control protocol

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
Area 1 (Org Structures) ──► Area 2 (Roster) ──► Area 3 (Meetings) ──► Area 4 (Runtime) ──► Area 5 (Tools)
                                              │
                                              ▼
                                         Area 9 (Anti-Bias)

Area 6 (Trading) ──► Area 7 (Dashboard)    ← independent track
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
