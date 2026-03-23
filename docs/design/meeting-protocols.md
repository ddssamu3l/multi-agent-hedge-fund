# Meeting Protocols

> How meetings run across all three organizational structures.
> Covers: meeting types, phase structure, speaking order, DM rules,
> turn management, and decision mechanics.
>
> Companion to `docs/design/runtime-documents.md` (what agents produce)
> and `docs/design/organizational-structures.md` (who reports to whom).

---

## Universal Rules (All Structures)

### Write-First Principle
Every meeting begins with all participating agents independently submitting
structured pre-meeting briefs (morning) or findings summaries (evening)
BEFORE any agent sees another's output. Briefs are revealed simultaneously
by the system. This is non-negotiable — the single most validated
anti-groupthink mechanism across all research (Delphi, GJP, NGT, Amazon
memos, Point72 pitches). See `docs/research/meeting-frameworks.md`.

### Turn Limits
- **100 turns max per meeting.** Hard ceiling.
- Agents have a natural exit mechanism — they can leave when they feel
  the conversation is done. No obligation to stay.
- System sends reminders: "50 turns remaining", "25 turns remaining",
  "10 turns remaining — wrap up."
- Most meetings will end naturally well before the limit. Some days
  there's nothing to say (15 turns). Other days Iran gets bombed (80 turns).

### Moderation
- **System code moderates.** No LLM moderator. Deterministic, zero cost.
- System manages: phase transitions, speaker selection, brief reveals,
  time reminders, vote collection, org log updates.

### Speaking Order
- **Weighted random with callout priority.**
  - Each agent has a base speaking weight.
  - Agents who haven't spoken recently get boosted (recency decay).
  - If an agent is directly referenced or challenged by name
    ("this contradicts @SEMI's thesis"), they get the next slot
    automatically. Direct callouts override the random selection.
  - Risk agent has a multiplier — speaks less frequently in general
    discussion but ALWAYS gets a turn on trade recommendations.
- **End-of-meeting check-in:** Before closing any meeting, system does
  a quick round: every agent gets one slot to say anything unsaid.
  Agents can pass. This guarantees quiet agents aren't silenced by
  the random selection.

### Why Not Round-Robin
Round-robin failed in the collab prototype experiments. It produces
unnatural conversation — agents speak because it's their turn, not
because they have something to say. First speakers anchor every
discussion. The most relevant voice on a topic might be speaker #9
and has to wait through 8 irrelevant turns. Weighted random with
callout priority solves all three problems.

### Risk Agent Presence
The Risk agent is present in EVERY meeting across all structures.
They observe, they speak on every trade recommendation, and they
can veto. Risk never sleeps.

---

## Structure A: The Council (Peer Network)

### Meeting Types

The Council has two daily meetings. No weekly or monthly special events.
Strategy, information sharing, and trade decisions all happen in the
daily cadence. If a major allocation shift is needed, agents raise it
in the evening meeting like any other recommendation.

#### Morning Meeting (Daily)

**Purpose:** Equal parts information sharing and strategy. The world
moves continuously — there's always something to talk about. If three
agents independently flagged the same signal overnight, the morning
meeting is where that convergence gets recognized and acted on.

**Who's in the room:** All 11 agents (10 domain + Risk).

**Phase structure:**

```
PHASE 1: BRIEF REVEAL (0 turns, system-managed)
  All 11 pre-meeting briefs revealed simultaneously.
  Every agent can now read every other agent's brief.
  No speaking yet — just reading.

PHASE 2: FLAGS & REACTIONS
  Agents respond to what they read in the briefs.
  Speaker selection: weighted random with callout priority.

  Each turn: react to another agent's brief or raise a
  strategic point. Must be specific — no "I agree with
  the general sentiment."

  This is where convergence gets spotted: "Three of us
  flagged credit tightening signals — that's not a
  coincidence. We should make this a priority today."

PHASE 3: STRATEGY & COORDINATION
  What is everyone researching today?
  Prevents duplicate work. Surfaces collaboration opps.
  Agents can propose DMs or private meetings:
  "I want to pull @CHINA and @OIL into a room to
  hash out the Hormuz scenario this afternoon."

PHASE 4: CHECK-IN
  System round: every agent gets one slot for anything
  unsaid. Pass is fine.

OUTPUT: Org log morning summary. Research assignments
logged. No trade decisions — that's the evening meeting.
```

#### Evening Meeting (Daily)

**Purpose:** Full debate and decision-making. This is where portfolio
decisions happen. DMs, private meetings, and pre-meetings during the
execution block are all preparation for making the most of this meeting.
Agents can and should band together, align positions, and build
coalitions ahead of time — but decisions involve the whole group.

**Who's in the room:** All 11 agents (10 domain + Risk).

**Phase structure:**

```
PHASE 1: FINDINGS REVEAL (0 turns, system-managed)
  All agents submit daily findings summaries.
  Revealed simultaneously. Write-first principle.

PHASE 2: PRESENTATIONS
  Agents with material findings present.
  (Agent can pass: "Nothing new, my brief stands.")

  Speaker selection: weighted random, but agents who
  flagged new recommendations get priority.

  Each presenting agent: 1-2 turns to present finding
  + structured recommendation. Must use the REC-* schema
  from runtime-documents.md.

PHASE 3: DEBATE
  For each recommendation on the table:
  - Counterparty test: "Who's on the other side?"
  - Kill condition review: "Is this measurable?"
  - Cross-domain challenge: "Does anyone's data conflict?"

  Disagreeing agents get priority slots via callout.
  Risk agent speaks on every recommendation.

  This is the full debate. Agents who did nemawashi
  during the day bring pre-formed arguments. Coalitions
  surface. Real disagreement happens here.

PHASE 4: VOTE (system-managed)
  Each recommendation gets a believability-weighted vote.
  All agents submit: {rec_id, support/oppose, confidence}

  Threshold: ≥ 0.60 weighted vote → APPROVED
             < 0.60 → REJECTED (stays personal track record)

  Risk veto: Risk agent can block any approved rec.
  Override requires ≥ 0.70 weighted supermajority.
  Risk veto is FINAL — no CIO to override in the Council.

PHASE 5: CHECK-IN
  System round: anything unsaid? Pass is fine.

OUTPUT: Org log evening summary. Approved recs → org
track record + conditional orders queued. Rejected recs →
personal track records only.
```

---

## Structure B: The Firm (4-Layer Pyramid)

### Meeting Types

The Firm has two daily meetings, structured around the hierarchy.
No weekly or monthly special events — the original Idea Dinner concept
is removed because the CIO is present in every meeting and makes
trade decisions daily.

**Key rule: CIO sits in ALL meetings silently.**
- The CIO listens to every sector standup and every evening synthesis.
- The CIO does NOT speak during sector standups (prevents HiPPO
  anchoring — Amazon juniors-first principle).
- The CIO speaks in the evening synthesis to approve or reject
  trade recommendations. This is the only time the CIO's voice
  is heard daily.
- The CIO can veto at any time (silent veto during standups
  surfaces as a rejected rec in the evening).

**Key rule: Risk Committee is present in every meeting.**
- Observes everything. Speaks on trade recommendations.
- Has full data access across all sectors (bypasses info barriers).

#### Morning Sector Standups (Daily, 3 Parallel Meetings)

**Purpose:** Within-sector information sharing and strategy. Each sector
runs its own morning meeting simultaneously.

**Who's in the room (per sector):**

| Sector | Agents | Sector Head |
|--------|--------|-------------|
| Tech & Supply Chain | SEMI, TECH, CRYPTO | TECH_HEAD |
| Macro & Rates | MACRO, LIQUIDITY, JAPAN | MACRO_HEAD |
| Geo & Resources | CHINA, OIL, GEOPOLITICS, MINERALS | GEO_HEAD |

Plus: CIO (silent, veto only) and Risk (silent, observing) in all three.

**Speaking order: Analysts first, sector head last.**
(Amazon juniors-first rule. Prevents sector head from anchoring.)

**Phase structure:**

```
PHASE 1: BRIEF REVEAL (0 turns, system-managed)
  Sector analysts' briefs revealed within the sector.
  (Analysts cannot see briefs from other sectors —
  information barrier at L1.)

PHASE 2: ANALYST REPORTS
  Each analyst presents their overnight findings.
  Speaking order: weighted random among analysts,
  but all analysts speak before the sector head.

  Direct callouts honored: if SEMI mentions something
  that affects TECH, TECH gets next slot.

PHASE 3: SECTOR HEAD SYNTHESIS
  Sector head speaks LAST. Summarizes what they heard.
  Can push back: "SEMI and TECH are saying contradicting
  things about AI capex — reconcile before evening."
  Assigns research directions for the day.

PHASE 4: CHECK-IN
  Quick round: anything unsaid?

OUTPUT: Sector standup summary in sector log.
No trade decisions — that's the evening meeting.
```

#### Evening Synthesis (Daily)

**Purpose:** Full debate and decision-making. All layers present.
Trade decisions happen here. The CIO has final say.

**Who's in the room:** All 15 agents. L1 analysts, L2 sector heads,
L3 CIO, L4 Risk Committee.

**Speaking order: Analysts → Sector Heads → Risk → CIO.**
Information flows UP before any downward signal.

**Phase structure:**

```
PHASE 1: FINDINGS REVEAL (0 turns, system-managed)
  All agents submit findings summaries.
  NOW analysts can see across sectors — the evening
  meeting breaks the information barrier for discussion.
  (But their morning research was done in isolation.)

PHASE 2: SECTOR PRESENTATIONS
  Each sector head presents their sector's key findings.
  Analysts can add context or correct their sector head.

  Order: sector heads present first (synthesized view),
  then analysts can speak to fill gaps or disagree.

PHASE 3: CROSS-SECTOR DEBATE
  This is where cross-domain connections surface.
  "SEMI's packaging bottleneck + MACRO's rate view +
  CHINA's demand data all point to the same thing."

  Agents from different sectors can now directly engage.
  Direct callouts honored.
  Risk speaks on every recommendation.

PHASE 4: RECOMMENDATIONS & CIO DECISION
  Agents present structured recommendations (REC-* schema).

  Unlike the Council's vote, the Firm uses CIO authority:
  - Agents propose recommendations
  - CIO approves or rejects each one (final say)
  - CIO states reasoning for each decision
  - CIO can override Risk Committee veto
    (goes on permanent record, graded in track record)
  - Risk Committee can flag but CIO decides

  All decisions are logged with full attribution:
  who proposed, who supported, who opposed, CIO reasoning.

PHASE 5: CHECK-IN
  Quick round: anything unsaid?
  Obligation to dissent (McKinsey rule): silence when
  you disagree is a failure mode. If your analysis
  conflicts with the CIO's decision, you MUST speak.

OUTPUT: Org log evening summary. CIO-approved recs →
org track record + conditional orders queued.
Rejected recs → personal track records.
```

### Information Barriers — Clarified

The L1 information barrier is about **default context**, not a
communication ban:

- **What analysts CAN'T see by default:** Other sectors' published
  analyses, briefs, and meeting transcripts. SEMI doesn't automatically
  get MACRO's morning brief in their context.
- **What analysts CAN do:** DM any agent in any sector. The barrier
  is what's in your feed, not who you can talk to. SEMI can DM MACRO
  about rate impacts on chip capex — that's nemawashi.
- **Evening meeting breaks the barrier:** When everyone's in the room
  together, cross-sector information flows freely.
- **Risk sees everything always.** No barriers apply to Risk.

---

## Structure C: The Model (Unified Synthesis)

### No Meetings

Structure C has no meetings, no DMs, no interaction of any kind.
This is the experimental control group — what happens when you
remove social dynamics entirely?

### Daily Signal Submission

Each agent independently produces structured signals after reading
their morning feed. Same REC-* schema as other structures, but
submitted directly to the optimizer instead of to a meeting.

```
DAILY FLOW (Structure C)
═══════════════════════════════════════
5:00 PM   Agent wakes, reads morning feed
5:00-5:30 Writes structured signals (REC-* format)
5:30      Signals submitted to optimizer
5:30-5:35 Optimizer runs (code, not LLM):
            - Collects all signals
            - Weights by believability
            - Manages correlation
            - Enforces risk constraints
            - Outputs portfolio adjustments
5:35      Agent updates journal, goes to sleep

No morning meeting. No evening meeting. No DMs.
No debate. No coalition-building. No nemawashi.
Pure independent signal generation + algorithmic
combination.
```

### What This Tests
- Does removing social dynamics produce cleaner signals?
- Does algorithmic synthesis outperform peer debate (A)
  and hierarchical filtering (B)?
- What is lost when agents can't challenge each other?

---

## DM & Private Meeting Protocol

### Two Communication Tools

**1. DM (Direct Message)** — Private 1-on-1 conversation.

**2. Private Meeting** — Multi-agent group chat. Initiating agent
picks who to invite. 2+ agents.

Both use request/accept:
- Initiating agent sends invitation with a topic line.
- Invited agent(s) accept or decline. No social cost to declining.
  "I'm deep in TSMC filings right now, pass." is perfectly fine.
- This prevents loud agents from blowing up everyone's context.

### Visibility Rules

| Structure | Who sees DMs/meetings? |
|-----------|----------------------|
| A (Council) | Risk agent sees all DM and meeting content |
| B (Firm) | Risk Committee sees all DM and meeting content |
| C (Model) | No DMs or meetings exist |

### Who Can Talk to Whom

| Structure | DM rules |
|-----------|----------|
| A (Council) | Anyone ↔ anyone. Full mesh. |
| B (Firm) | Anyone ↔ anyone. Info barriers are about default context, not communication bans. SEMI can DM MACRO. |
| C (Model) | No communication. |

### When DMs/Meetings Happen

During the execution block (6:30-9:00 PM in the daily lifecycle).
Not during official meetings — agents should be present and engaged
in the morning and evening meetings, not side-chatting.

### What DMs Are For

DMs and private meetings are preparation for the evening meeting:
- **Nemawashi:** Test ideas privately before proposing publicly.
  Build coalitions. Identify objections early.
- **Cross-domain investigation:** "SEMI here — I'm seeing packaging
  constraints. @CHINA, what's your read on Chinese substrate supply?"
- **Conflict resolution:** Hash out a disagreement bilaterally
  before it consumes group meeting time.
- **Research collaboration:** "Let's look at Hormuz scenario
  together — I'll cover military angle, you cover oil flows."

Agents should know they can and should use DMs strategically.
Pre-aligning with allies, testing arguments, and gathering data
from other domains before the evening meeting is explicitly
encouraged. This is how real firms work — the meeting is where
decisions get ratified, not where ideas are born.

---

## Decision Mechanics Summary

### Structure A (Council) — Democratic

```
Agent proposes structured rec (REC-* schema)
  → Group debates in evening meeting
  → All 11 agents vote: {rec_id, support/oppose, confidence}
  → System computes believability-weighted result
  → ≥ 0.60 weighted: APPROVED (org decision, real portfolio)
  → < 0.60 weighted: REJECTED (personal track record only)

Risk veto:
  → Risk blocks approved rec with stated reasoning
  → Override requires ≥ 0.70 weighted supermajority
  → Risk veto is FINAL in the Council (no CIO to override)
```

### Structure B (Firm) — Authoritarian

```
Agent proposes structured rec (REC-* schema)
  → Sector discusses in evening meeting
  → CIO approves or rejects (final say)
  → CIO states reasoning, logged permanently
  → Approved: org decision, real portfolio
  → Rejected: personal track record only

Risk veto:
  → Risk Committee can block with stated reasoning
  → CIO CAN override Risk (unlike Council)
  → Override goes on permanent record
  → Override graded in org track record
  → This is a genuine topology difference being tested:
    does CIO ability to overrule risk help or hurt?
```

### Structure C (Model) — Algorithmic

```
Agent submits structured signal (REC-* schema)
  → Optimizer collects all signals daily
  → Weights by believability scores
  → Manages correlation (5 semi longs ≠ 5 independent bets)
  → Enforces hard risk constraints (max position, sector, drawdown)
  → Outputs portfolio (org decision, real portfolio)
  → No voting. No approval. No veto. Pure math.

Risk is embedded as optimizer constraints:
  → Max single position: 10%
  → Max sector exposure: 30%
  → Max drawdown trigger: -15% → reduce all by 50%
  → Max correlated pair: 15% combined at >0.7 correlation
  → Not overridable. Hard-coded.
```

---

## Cost Estimates (Updated)

| Structure | Daily Meeting Turns | Est. Daily Cost | Est. Monthly Cost |
|-----------|-------------------|----------------|------------------|
| A (Council) | ~40-80 (morning + evening) | $1.50-3.00 | $45-90 |
| B (Firm) | ~50-90 (3 standups + evening) | $2.00-3.50 | $60-105 |
| C (Model) | 0 (no meetings) | $0.30-0.50 (signals only) | $9-15 |
| **Combined** | | | **$114-210/month** |

Plus DMs and private meetings during execution blocks (~$0.50-1.50/day
across A and B), data ingestion (~$3.60/month), and individual agent
research during execution (~$1-2/day per structure).

**Estimated total: $180-300/month for all three structures.**
