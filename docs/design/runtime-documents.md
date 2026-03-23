# Runtime Document Schemas

> Schemas for the three document types agents produce during their daily cycle:
> the pre-meeting brief (mini-thesis), the personal journal, and the org log.
> Informed by research in `docs/research/analyst-journal-frameworks.md` and
> `docs/research/meeting-frameworks.md`.

---

## Design Principles

1. **Write-first:** Agents commit positions to paper before any group discussion.
   This is the single most validated anti-groupthink mechanism across Delphi,
   GJP, NGT, Amazon memos, and Point72 pitches.

2. **Findings must connect to decisions:** Talking points without actionable
   recommendations are just words. Every observation must trace to "so what
   should we buy/sell/hold?" (the "one layer deeper" mentality applied to
   communication — see reasoning-examples.md).

3. **Conviction is a number, not a word:** "Bullish" is meaningless. 0.78 is
   auditable. Every stance includes a numeric confidence that feeds
   believability-weighted aggregation.

4. **Everything has a kill condition:** No thesis, trigger, or prediction lives
   forever. Explicit conditions for invalidation, pre-committed before entry.

5. **Append-only for evidence and predictions:** Never overwrite history.
   Timestamp all additions. The audit trail IS the research.

6. **Machine-parseable where possible:** Triggers, predictions, and trade
   recommendations should be structured enough for automated monitoring.

7. **Track record is identity:** Every important call is logged, validated
   over time, and graded on BOTH process quality and outcome quality (Dalio's
   principle: separate good decisions from good luck). Track records exist at
   two layers — personal (each agent's call history) and organizational (the
   group's collective decision history). This is what feeds believability
   weighting and what enables agents to learn from their own history.

8. **Systemic patterns over individual outcomes:** A single wrong call is
   noise. Three wrong calls in the same pattern is signal. The track record
   system surfaces recurring failure modes at both personal and org level.

9. **Personal calls ≠ org decisions.** Agent recommendations are mock trades
   in a personal paper portfolio — tracked against real market data for
   self-calibration and believability scoring, but carrying zero portfolio
   weight. Only calls that pass the group's decision process (vote, CIO
   approval, or optimizer selection) become org decisions that move the
   actual portfolio. This separation lets agents take analytical risks
   freely. A rejected call that turns out right is the system's most
   valuable signal — it means the group is underweighting that agent.

---

## Document 1: Pre-Meeting Brief (Mini-Thesis)

Written by every agent BEFORE entering any group meeting. Locked before
discussion begins. No agent sees another's brief until all are submitted.

This serves two purposes: (1) anchors the agent's independent thinking against
groupthink, and (2) gives the group structured material to discuss rather than
free-form chat.

```
PRE-MEETING BRIEF
═══════════════════════════════════════════════════════════
Agent: [name]
Date: [YYYY-MM-DD]
Meeting: [morning / evening / monthly-allocation / ad-hoc]

─── SECTION 1: WHAT I SAW ───────────────────────────────

OVERNIGHT EVENTS (1-3 items most relevant to my domain):
  1. [Event + source + why it matters]
  2. [Event + source + why it matters]
  3. [Event + source + why it matters]

DATA CHANGES:
  [Key metrics in my domain that moved since last session.
   Not a data dump — only items that change my analysis.
   Example: "TSMC Feb revenue: $6.2B (+12% YoY), third
   consecutive month of acceleration. Confirms my HBM
   demand thesis."]

─── SECTION 2: MY CURRENT STANCE ────────────────────────

ACTIVE THESES (status check):
  | Thesis ID | Title              | Conviction | Change | Kill Status |
  |-----------|--------------------|-----------:|--------|-------------|
  | TH-005    | AI semi supercycle | 0.82       | unch   | holding     |
  | TH-011    | JPY carry unwind   | 0.65       | ↓ 0.70 | watching    |

STANCE CHANGES SINCE LAST MEETING:
  [If any conviction changed, state WHAT changed and WHY.
   "Reduced JPY carry unwind conviction from 0.70 to 0.65
   because BoJ rhetoric softened in March minutes."]

KILL CONDITION CHECK:
  [Are any active theses approaching their kill conditions?
   "TH-011 kill condition is '10Y JGB yield drops below
   0.5%.' Currently at 0.62%. Getting close — flagging."]

─── SECTION 3: WHAT THE GROUP IS MISSING ─────────────────

FLAG:
  [One thing I believe the group is underweighting or
   overlooking. This is the most important section.
   Forces the agent to think beyond its own theses and
   contribute to group intelligence.

   Example: "Everyone is focused on NVIDIA earnings but
   nobody is watching ABF substrate pricing. Ajinomoto
   raised prices 15% this week. This is a leading indicator
   of advanced packaging capacity constraints that will
   hit ALL AI chip makers in Q3. We should be looking at
   packaging bottlenecks, not just GPU demand."]

─── SECTION 4: ACTIONABLE RECOMMENDATIONS ────────────────

[Every finding must trace to a concrete recommendation.
 Not "oil is interesting" but structured output that the
 system can automatically track.

 0-3 recommendations per brief. Zero is fine if nothing
 actionable emerged. Agents should NOT force recs to fill
 the section. But any rec MUST use this exact schema —
 the system code parses these fields to create track
 record entries automatically.

 IMPORTANT: These are PERSONAL recommendations — they
 carry NO portfolio weight on their own. They are the
 agent's individual analytical position, tracked in their
 personal track_record.md for self-calibration and
 believability scoring. Think of it as the agent's paper
 portfolio.

 To become an ORG DECISION that moves the actual portfolio,
 a recommendation must pass through the group's decision
 process:
   - Structure A (Council): weighted vote ≥ threshold
   - Structure B (Firm): CIO approval after sector head synthesis
   - Structure C (Model): optimizer selects from signal pool

 This split means an agent builds their track record
 regardless of whether the group adopts their calls.
 High personal accuracy + low org adoption = "we should
 listen to this agent more." Low personal accuracy +
 high org adoption = "we need to reconsider this agent's
 influence."]

RECOMMENDATION:
  rec_id:         [auto-assigned: REC-{agent}-{date}-{seq}]
  action:         BUY | SELL | HOLD | HEDGE | WATCH
  asset:          [ticker symbol, e.g. "EPD"]
  asset_name:     [full name, e.g. "Enterprise Products Partners"]
  size_pct:       [float, suggested % of portfolio, e.g. 2.0]
  confidence:     [float, 0.0-1.0, e.g. 0.72]
  timeframe:      [duration string, e.g. "3 months"]
  entry_price:    [float or null if WATCH, e.g. 29.50]
  target_price:   [float, e.g. 35.00]
  stop_price:     [float, hard exit, e.g. 26.00]
  thesis_id:      [link to active thesis, e.g. "TH-019"]
  variant_view:   [string — how this differs from consensus]
  reasoning:      [string — causal chain from event to trade]
  kill_condition:  [string — specific, measurable, e.g.
                    "Hormuz reopens to commercial shipping"]
  kill_metric:    [string — data source to check, e.g.
                    "Lloyd's List Hormuz transit count"]
  counterparty:   [string — who disagrees and why]
  tags:           [list of strings, e.g. ["energy", "geopolitics",
                    "middle-east", "infrastructure"]]

  # ── SYSTEM-MANAGED FIELDS (agent never writes these) ──
  status:         PENDING | APPROVED | REJECTED | EXECUTED |
                  CLOSED_WIN | CLOSED_LOSS | KILLED | EXPIRED
  created_at:     [timestamp, auto]
  approved_at:    [timestamp, set when group votes to approve]
  executed_at:    [timestamp, set when order fills]
  closed_at:      [timestamp, set on exit]
  exit_price:     [float, set on exit]
  return_pct:     [float, computed on exit]
  process_grade:  [A-F, set during validation]
  outcome_grade:  [A-F, set during validation]
  lesson:         [string, set during agent reflection]

EXAMPLE (good):
  rec_id:         REC-GEO-20260322-01
  action:         BUY
  asset:          EPD
  asset_name:     Enterprise Products Partners
  size_pct:       2.0
  confidence:     0.72
  timeframe:      3 months
  entry_price:    29.50
  target_price:   36.00
  stop_price:     26.00
  thesis_id:      TH-019
  variant_view:   "Market prices Hormuz risk into oil futures
                   but not into pipeline infrastructure operators
                   who benefit regardless of actual closure"
  reasoning:      "US strikes Iran → Strait of Hormuz risk →
                   oil pipeline alternatives at max capacity →
                   EPD operates largest US crude pipeline network
                   → volume surge even from elevated risk premium,
                   doesn't require actual closure"
  kill_condition:  "Hormuz risk fully de-escalated, Iran deal"
  kill_metric:    "US-Iran diplomatic status, Hormuz transit data"
  counterparty:   "EPD already trading near fair value for current
                   volumes. Pipeline utilization may not increase
                   as much as expected if shippers have long-term
                   contracts locking routes through Hormuz."
  tags:           ["energy", "geopolitics", "infrastructure"]

EXAMPLE (rejected — what NOT to do):
  action:         BUY
  asset:          ???          ← no ticker
  size_pct:       ???          ← no sizing
  confidence:     ???          ← no number
  reasoning:      "Middle East tensions rising, consider
                   energy exposure"  ← no causal chain
  kill_condition: "things calm down"  ← not measurable

─── SECTION 5: TODAY'S PLAN ──────────────────────────────

RESEARCH PRIORITIES:
  1. [What I plan to investigate today and why]
  2. [What I plan to investigate today and why]

MEETINGS REQUESTED:
  [Any private meetings or DMs I want to initiate today.
   Example: "Want to DM @CHINA about Hormuz impact on
   Chinese crude imports — their demand data matters
   for my pipeline thesis."]

PENDING FROM YESTERDAY:
  [Carryover items from last session's open threads]

═══════════════════════════════════════════════════════════
```

### One Layer Deeper in Recommendations

The recommendation section enforces the "one layer deeper" reasoning pattern.
The chain should always go: **Event → First-order effect → Second-order effect
→ Specific asset → Why the market hasn't priced this → Trade.**

```
EXAMPLE: Iran Strike Scenario (GEOPOLITICS agent)

Layer 0 (what everyone knows):
  "US strikes Iran. Oil up."

Layer 1 (first-order):
  "Strait of Hormuz at risk. ~20% of global oil transits
  there. Oil futures spike."

Layer 2 (second-order):
  "Alternative routes to Hormuz: Saudi East-West pipeline
  (5 Mb/d capacity), UAE Habshan-Fujairah (1.5 Mb/d).
  Both at partial capacity. Will run at max."

Layer 3 (third-order — the trade):
  "Intelligence was key to strike success. Palantir's
  Gotham platform was used for force distribution analysis
  and strike simulation. This is a government contract
  catalyst that the market won't price until the next
  10-Q shows DoD revenue spike. BUY PLTR."

  "Enterprise Products Partners (EPD) operates largest
  US crude pipeline network. If Hormuz disrupted, US
  export infrastructure becomes critical. EPD volume
  and tolling fees surge. BUY EPD."

Layer 4 (cross-domain — requires other agents):
  "Hormuz closure → Chinese crude imports disrupted →
  PBoC forced to release strategic petroleum reserves →
  Chinese industrial activity temporarily constrained →
  TSMC power costs rise (Taiwan is energy-dependent) →
  semiconductor margin pressure next quarter."
  ← This is the Ajinomoto-style cross-domain connection
  that only emerges when GEOPOLITICS talks to CHINA talks
  to SEMI.
```

This reasoning chain — from event to specific tradeable insight through
multiple layers — is what separates our agents from generic financial
chatbots. The pre-meeting brief's recommendation section FORCES agents to
trace all the way down to a concrete trade, not stop at "oil goes up."

---

## Document 2: Personal Journal (Agent Daily Log)

Each agent maintains a personal journal that serves as their working memory.
This is NOT the same as identity.md or memory.md — those are persistent
identity documents. The journal is the daily operational log, closer to
recent.md but with more structure.

Updated throughout the daily cycle. Sections have different update cadences.

```
AGENT JOURNAL — [Agent Name]
═══════════════════════════════════════════════════════════

─── § DAILY LOG ──────────────────────────────────────────
[Updated every session. Most recent entry first.]

[2026-03-22]
  Morning brief: [link to pre-meeting brief]
  Key findings today: [2-3 bullet summary of research]
  Stance changes: [any conviction changes with reasoning]
  Trades recommended: [any trade recs made today]
  Open threads: [what carries to tomorrow]
  Self-assessment: [hot/cold/neutral streak. Druckenmiller-
    inspired: "Am I seeing the ball right now?"]

[2026-03-21]
  [Previous day, progressively compressed over time]

─── § ACTIVE THESES ──────────────────────────────────────
[Long-lived. Updated when evidence arrives or conviction
 shifts. This is the core of the agent's intellectual work.]

THESIS: [ID] [Title]
  Status:     [hypothesis / research / active / mature / dead]
  Created:    [date]
  Conviction: [0.0-1.0] (history: 0.60→0.72→0.82)
  Timeframe:  [expected duration]

  Statement:
    [1-3 sentence thesis — specific, falsifiable]

  Variant View:
    [How does this differ from market consensus?
     What does the market have WRONG?]

  Key Assumptions:
    1. [Assumption] — Status: [confirmed/holding/watching/at-risk]
       Kill: [specific condition that invalidates this assumption]
    2. [Assumption] — Status: [...]
       Kill: [...]

  Evidence Log (append-only):
    | Date       | Evidence                  | Impact           |
    |------------|---------------------------|------------------|
    | 2026-03-10 | TSMC Feb rev +12% YoY     | Confirms demand  |
    | 2026-03-18 | N. Virginia power delays   | Risk to thesis   |

  Related Positions: [tickers held based on this thesis]
  Related Theses: [links to other theses that interact]
  Counterparty View: [the bear case, stated as strongly as
    the bull case — Counterparty Test from reasoning rules]

  Next Review: [date or catalyst trigger]

─── § WATCHLIST ──────────────────────────────────────────
[Three tiers of attention. Professional analyst standard.]

TIER 1 — ACTIVE POSITIONS (check daily):
  | Ticker | Dir  | Entry  | Current | P&L    | Kill Condition     | Next Catalyst |
  |--------|------|--------|---------|--------|--------------------|---------------|
  | NVDA   | Long | $142   | $155    | +9.2%  | TSMC rev declines  | Apr 15 event  |
  | TSM    | Long | $162   | $178    | +9.9%  | <$150 for 5 days   | Mar rev data  |

TIER 2 — READY LIST (check weekly):
  | Ticker | Thesis Link | Entry Trigger              | Target Price | Expiry     |
  |--------|-------------|----------------------------|-------------|------------|
  | ASML   | TH-005      | Pullback to €880 support   | €880-€900   | 2026-06-01 |
  | EPD    | TH-019      | Hormuz escalation confirmed | Market      | 2026-05-01 |

TIER 3 — RESEARCH PIPELINE (check monthly):
  | Name/Theme          | Initial Observation          | Status     | Priority |
  |---------------------|------------------------------|------------|----------|
  | Ajinomoto ABF       | 15% price hike, monopoly     | screening  | high     |
  | Uranium / SMR cycle | AI datacenter power demand   | deep-dive  | medium   |

─── § IF-THEN TRIGGERS ──────────────────────────────────
[Armed triggers waiting for conditions. Machine-parseable.]

TRIGGER: [ID]
  Domain:    [agent's domain]
  Type:      [threshold / trend / event / divergence / composite / time]

  IF:
    [Specific, measurable condition(s)]
    [Can be compound: A AND B AND NOT C]

  THEN:
    [Specific action(s) to take]

  Rationale: [Why this trigger matters — the causal chain]
  Source:    [Where to check this data]
  Frequency: [How often to check]
  Status:   [armed / triggered / expired / cancelled]
  Expiry:   [Date after which reassess]
  Last Checked: [Date + result]

─── § PREDICTIONS ────────────────────────────────────────
[Structured, timestamped, probability-weighted. Scored on
 resolution. Auto-creates a track record entry. This feeds
 the believability-weighting system.]

PREDICTION SCHEMA (structured):
  pred_id:        [auto: PRED-{agent}-{YYYYMMDD}-{seq}]
  created_at:     [timestamp]
  question:       [string — specific, time-bounded, resolvable.
                   GOOD: "Will Fed cut rates ≥50bp before Sep 2026?"
                   BAD: "Will the economy slow down?"]
  resolution_date: [date — when this CAN be graded]
  probability:    [float 0.0-1.0]
  direction:      [BULLISH | BEARISH | NEUTRAL — for the asset/topic]
  asset:          [ticker or "MACRO" or "GEO" etc.]
  reasoning:      [string — why this probability, not higher/lower]
  key_assumptions: [list of strings — what must hold]
  tags:           [list — domain tags for calibration analysis]

  # ── AGENT-MANAGED: UPDATE LOG ──
  updates:        [append-only list:
                    {date, new_p, delta, reason}
                   e.g. {2026-03-21, 0.32, -0.03, "Strong jobs report"}]

  # ── SYSTEM-MANAGED: RESOLUTION ──
  status:         OPEN | NEEDS_VALIDATION | RESOLVED
  outcome:        YES | NO | AMBIGUOUS
  brier_score:    [float, computed: (forecast - outcome)^2]
  resolved_at:    [timestamp]

─── § ERROR LOG ──────────────────────────────────────────
[Dalio-inspired. The punishment is for NOT logging, not
 for being wrong. Structured reflection on misses.]

ERROR: [ID]
  Date:     [when the error was recognized]
  Type:     [prediction miss / thesis failure / process failure /
             late entry / premature exit / missed signal]

  What happened:
    [Factual description of what went wrong]

  Root cause:
    [Not surface-level. Why did this REALLY happen?
     Was it a data gap, analytical error, conviction failure,
     groupthink, or bad luck despite good process?]

  Lesson / New Principle:
    [What decision rule should prevent this in the future?
     This is how the agent evolves — errors become principles
     that modify future behavior.]

  Severity: [1-5, where 5 = portfolio-impacting]

─── § TRACK RECORD ───────────────────────────────────────
[Separate file: track_record.md. NOT in the daily journal.
 Append-only — never delete or edit past entries. Only add
 resolution data when outcomes become clear.

 THIS IS THE AGENT'S PERSONAL PAPER PORTFOLIO. These calls
 carry NO real portfolio weight. They are mock trades —
 the agent's own analytical positions, tracked against
 real market data for self-calibration and believability
 scoring. Like a junior analyst's shadow book.

 The personal track record serves three purposes:
 1. SELF-CALIBRATION: "Am I actually good at this?"
    Agent sees its own accuracy, streaks, blind spots.
 2. BELIEVABILITY INPUT: Personal accuracy on domain X
    determines voting weight when the group decides on X.
 3. ANALYTICAL FREEDOM: Agents can take positions the
    group rejected. If the agent was right and the group
    was wrong, that's evidence the group should listen
    more. If the agent keeps making calls the group
    rejects and those calls lose, that's evidence too.

 An agent's personal rec can have three fates:
   ┌─ GROUP ADOPTS → becomes ORG DECISION (real portfolio)
   │                  tracked in BOTH personal + org records
   │
   ├─ GROUP REJECTS → stays personal only (mock trade)
   │                   tracked in personal record against
   │                   real market data. Did the group miss?
   │
   └─ AGENT DOESN'T PROPOSE → agent can still log personal
                               calls in their journal that
                               they chose not to bring to
                               the group (low confidence,
                               still researching, etc.)

 CREATION IS AUTOMATIC. When an agent produces a structured
 recommendation (REC-*), prediction (PRED-*), or thesis
 declaration (TH-*), the system code parses those structured
 fields and creates a track record entry. The agent cannot
 choose what gets tracked. Every call is on the record.

 VALIDATION IS TRIGGERED BY CODE, FILLED BY AGENT + AUDITOR.
 When monitoring detects that a condition resolved (price hit,
 time expired, kill triggered), code sets status to
 NEEDS_VALIDATION and queues it. The agent reflects during
 its next cycle. The context processor audits the reflection.]

ENTRY SCHEMA (structured — every field explicit):
  # ── CREATED AUTOMATICALLY BY SYSTEM CODE ──
  call_id:        [auto: CALL-{agent}-{YYYYMMDD}-{seq}]
  source_id:      [the rec/prediction/thesis ID that spawned this]
  source_type:    REC | PREDICTION | THESIS | RISK_FLAG | EXIT_CALL
  agent:          [agent name]
  created_at:     [timestamp]
  statement:      [exact claim, copied verbatim from source]
  action:         [BUY | SELL | HOLD | HEDGE | WATCH | FLAG]
  asset:          [ticker or "N/A" for macro calls]
  confidence:     [float 0.0-1.0, copied from source]
  timeframe:      [duration, copied from source]
  kill_condition:  [copied from source]
  kill_metric:    [copied from source — how to check]
  reasoning:      [copied from source]
  context_snapshot: [key market data at time of call, auto-captured:
                     {SPX, VIX, DXY, 10Y, relevant sector ETF}]
  adopted_by_org: [bool — did the group approve this?]
  org_decision_id: [DEC-* ID if adopted, null if not]
  status:         PENDING

  # ── FILLED BY CODE WHEN CONDITIONS MET ──
  status:         PENDING → NEEDS_VALIDATION
  resolved_at:    [timestamp when condition detected]
  resolution_trigger: PRICE_TARGET | STOP_HIT | KILL_TRIGGERED |
                      TIME_EXPIRED | EVENT_OCCURRED | MANUAL
  outcome_data:   [factual, auto-captured by code:
                    {asset_price_at_resolution, return_pct,
                     time_held, market_return_same_period}]

  # ── FILLED BY AGENT DURING REFLECTION ──
  status:         NEEDS_VALIDATION → VALIDATED
  outcome:        CORRECT | PARTIALLY_CORRECT | WRONG |
                  TOO_EARLY | TOO_LATE | AMBIGUOUS
  what_right:     [string — specific elements that held up]
  what_wrong:     [string — specific elements that didn't]
  process_grade:  A | B | C | D | F
                  [A = right for right reasons
                   B = right but some reasoning flawed
                   C = wrong but reasoning was defensible
                   D = wrong and reasoning was flawed
                   F = wrong and should have known better]
  outcome_grade:  A | B | C | D | F
  lesson:         [string — one concrete principle extracted]

  # ── FILLED BY CONTEXT PROCESSOR (audit) ──
  audit_grade:    [A-F — ctx processor's independent assessment]
  audit_note:     [if agent's self-grade diverges from auditor's
                   grade, note why. e.g. "Agent graded self B but
                   kill condition was clearly met 2 weeks before
                   exit. Process grade should be D — failed to
                   monitor own kill condition."]
  status:         VALIDATED → CONFIRMED (permanent record)

TRACK RECORD SUMMARY (auto-computed):
  Total calls: [n]
  Resolved: [n] | Pending: [n]
  Adopted by org: [n] | Personal only: [n]

  By type:
    | Type         | Correct | Wrong | Accuracy | Avg Conf | Calibration |
    |--------------|---------|-------|----------|----------|-------------|
    | Thesis       | 5       | 2     | 71.4%    | 0.74     | +3% overconf|
    | Prediction   | 12      | 6     | 66.7%    | 0.68     | well-cal    |
    | Trade rec    | 8       | 3     | 72.7%    | 0.71     | +5% overconf|
    | Risk flag    | 4       | 1     | 80.0%    | 0.65     | underconf   |

  By domain:
    | Domain          | Calls | Accuracy | Brier  | Trend       |
    |-----------------|-------|----------|--------|-------------|
    | Semiconductors  | 18    | 77.8%    | 0.15   | stable      |
    | Macro spillover | 6     | 50.0%    | 0.28   | declining   |
    | Cross-domain    | 3     | 66.7%    | 0.22   | too few     |

  Personal vs Org-Adopted (THE key comparison):
    | Category       | Calls | Accuracy | Avg Return | Notes              |
    |----------------|-------|----------|------------|--------------------|
    | Adopted by org | 11    | 72.7%    | +8.2%      | moved real portfolio|
    | Rejected by org| 7     | 71.4%    | +6.5%      | would have worked! |
    | Personal only  | 11    | 63.6%    | +2.1%      | lower conviction   |

    [This table answers: "Is the group missing my good calls?"
     If rejected calls outperform adopted calls, the group
     should increase this agent's voting weight.
     If personal-only calls underperform, the agent is right
     to keep low conviction ideas out of group discussion.]

  Notable calls (best and worst):
    BEST:  [ID] — "Called TSMC CoWoS shortage 3 months before
            market priced it. Confidence 0.85. Resulted in +22%
            on TSMC position." [2026-04-15]
    WORST: [ID] — "Called Japan carry trade unwind at 0.75
            confidence. BoJ reversed course, yen weakened further.
            Process grade: D — ignored BoJ's explicit forward
            guidance." [2026-05-20]
    BEST REJECTED: [ID] — "Recommended SHORT ARKK at 0.68
            confidence. Group rejected (vote 0.42). ARKK dropped
            22% over next 3 months. Group should have listened."

  Streaks:
    Current: [3 correct in a row / 2 wrong in a row / mixed]
    Longest winning: [n calls, dates]
    Longest losing:  [n calls, dates]
    [Druckenmiller: "My number one job is to know when I'm hot
     and when I'm not."]

─── § SELF-ASSESSMENT ────────────────────────────────────
[Updated weekly. Informed by track record data above.
 Druckenmiller's "streak" tracking + Soros's pain signal
 + Dalio's believability feedback.]

Recent accuracy:
  Last 10 resolved predictions: [X/10 directionally correct]
  Rolling Brier score (30-day): [X.XX]
  Accuracy trend: [improving / stable / declining]

Domain confidence:
  Primary domain: [X/5] — [brief note on why]
  Adjacent domains: [list with scores]

Analytical state:
  Streak: [hot / cold / neutral]
  [If cold: "My semiconductor calls have been off — downweighting
   my own confidence. MACRO has been sharp recently, giving their
   rate views more weight in my cycle model."]

  Highest conviction thesis: [link + conviction level]
  Biggest uncertainty: [what's keeping me up at night]

Anomaly flags:
  Accuracy deteriorating?       [Y/N + details]
  Any thesis near kill?         [Y/N + which]
  Unresolved disagreements?     [Y/N + with whom, about what]

═══════════════════════════════════════════════════════════
```

### Journal vs. Memory Documents

The journal is NOT a replacement for the three-document identity model
(identity.md / memory.md / recent.md). Here's how they relate:

```
JOURNAL (operational)          MEMORY SYSTEM (persistent)
─────────────────────          ──────────────────────────
Daily log entries         ──►  recent.md (compressed summary)
Active theses             ──►  memory.md § Current Goals
Watchlist                 ──►  memory.md § Current Goals
If-then triggers          ──►  memory.md § Current Goals (armed)
Predictions               ──►  memory.md § Track Record (resolved)
Error log                 ──►  memory.md § Past Work (lessons)
Track record (resolved)   ──►  memory.md § Track Record (notable calls)
Track record (lessons)    ──►  identity.md § Evolved Traits (permanent shifts)
Self-assessment           ──►  identity.md § Evolved Traits (long-term shifts)

The journal is the WORKING document. The memory system is the
DISTILLED document. A context processor (cheap LLM) promotes
significant journal entries into the memory system at end of
each session.
```

---

## Document 3: Organization Log (Group-Level)

Each organizational structure (Council, Firm, Model) maintains its own log.
This is the collective memory of the group — what was discussed, what was
decided, and what the group's strategy is.

```
ORGANIZATION LOG — [Structure Name]
═══════════════════════════════════════════════════════════

─── § DAILY RECORD ───────────────────────────────────────

[2026-03-22]
  Session: Daily cycle #47
  Agents present: [list]

  MORNING MEETING SUMMARY:
    Key events discussed:
      1. [Event + who raised it + group assessment]
      2. [Event + who raised it + group assessment]

    Flags raised (from pre-meeting briefs):
      - @SEMI flagged ABF substrate pricing → group assigned
        @SEMI to deep-dive packaging bottleneck impact
      - @GEOPOLITICS flagged Hormuz risk → group assigned
        @OIL and @GEOPOLITICS to collaborate on scenario analysis

    Research directions agreed:
      1. [Task + assigned agent(s) + expected output]
      2. [Task + assigned agent(s) + expected output]

    Disagreements surfaced:
      - @MACRO vs @LIQUIDITY on rate cut timing (unresolved,
        carrying to evening meeting)

  EVENING MEETING SUMMARY:
    Findings presented:
      - @SEMI: [1-2 sentence finding summary + recommendation]
      - @GEOPOLITICS: [1-2 sentence finding summary + recommendation]
      [Only agents with material findings present]

    Recommendations debated:
      | Rec      | Agent    | Action    | Asset | Conf | Group Vote | Outcome |
      |----------|----------|-----------|-------|------|------------|---------|
      | REC-047a | @GEO     | BUY       | EPD   | 0.72 | 0.68 wtd   | APPROVED|
      | REC-047b | @SEMI    | INCREASE  | NVDA  | 0.85 | 0.71 wtd   | APPROVED|
      | REC-047c | @MACRO   | SELL      | TLT   | 0.60 | 0.45 wtd   | REJECTED|

    Key debates:
      [Summary of the most important disagreements and how
       they were resolved or tabled. Include WHO argued WHAT.]

    Risk agent assessment:
      @RISK: [Portfolio-level risk assessment. Correlation
      flags, concentration warnings, Minsky stage update.]

  ORDERS PLACED:
    | Order ID | Ticker | Action | Size  | Limit  | Kill    | Status  |
    |----------|--------|--------|-------|--------|---------|---------|
    | ORD-142  | EPD    | BUY    | 2.0%  | $29.50 | $35     | queued  |
    | ORD-143  | NVDA   | BUY    | 1.5%  | $160   | $200    | queued  |

─── § PORTFOLIO SNAPSHOT ─────────────────────────────────
[Updated daily after evening meeting.]

  Total Value: $1,024,300 (+2.43% inception)
  Cash: $312,100 (30.5%)

  POSITIONS:
    | Ticker | Dir  | Size % | Entry  | Current | P&L     | Thesis   | Kill         |
    |--------|------|--------|--------|---------|---------|----------|--------------|
    | NVDA   | Long | 8.2%   | $142   | $155    | +9.2%   | TH-005   | TSMC rev ↓   |
    | TSM    | Long | 6.1%   | $162   | $178    | +9.9%   | TH-005   | <$150 5 days |
    | GLD    | Long | 4.0%   | $198   | $205    | +3.5%   | TH-012   | Real yield>2%|

  SECTOR EXPOSURE:
    Technology: 18.3%
    Energy: 2.0%  (pending EPD fill)
    Commodities: 4.0%
    Cash: 30.5%
    [Max sector: 30%. Max single position: 10%.]

  RISK METRICS:
    Correlation (avg pairwise): 0.42
    Max drawdown (inception): -3.1%
    Sharpe ratio (annualized): [calculated after 30+ days]
    Minsky stage assessment: [hedge / speculative / ponzi]

─── § STRATEGY ───────────────────────────────────────────
[Updated when the group's strategic direction changes.
 Not daily — only when there's a real shift.]

  Current strategic posture: [risk-on / risk-off / neutral]
  Primary thesis driving allocation: [link to dominant thesis]

  Strategic priorities (updated [date]):
    1. [Priority + rationale + timeframe]
    2. [Priority + rationale + timeframe]
    3. [Priority + rationale + timeframe]

  Strategic debates (unresolved):
    - [Debate topic + agents on each side + what would resolve it]

─── § COLLECTIVE PREDICTIONS ─────────────────────────────
[Group-level predictions, aggregated from individual agents.
 Tracked for the organization's collective calibration.]

  | Pred ID | Question                         | Group P | Method    | Due        |
  |---------|----------------------------------|---------|-----------|------------|
  | GP-012  | Fed cuts ≥50bp before Sep 2026?  | 0.32    | Ext. mean | 2026-09-01 |
  | GP-015  | TSMC rev growth stays >10% YoY?  | 0.78    | Wtd vote  | 2026-06-30 |
  | GP-018  | Brent oil >$100 before Jun 2026? | 0.25    | Ext. mean | 2026-06-01 |

  Resolved predictions (last 30):
    Accuracy: 14/22 directionally correct (63.6%)
    Brier score: 0.21
    Calibration: slight overconfidence in 60-80% range

─── § DECISION LOG ───────────────────────────────────────
[Every portfolio decision with rationale. Audit trail.]

  DECISION: [ID]
    Date: [when decided]
    Type: [entry / exit / rebalance / hedge / hold]
    Action: [specific trade]
    Rationale: [why, linked to thesis and meeting discussion]
    Proposed by: [agent]
    Supported by: [agents + weighted vote]
    Opposed by: [agents + reasoning]
    Risk assessment: [risk agent's take]
    Outcome: [filled after position closed]

─── § BELIEVABILITY SCORES ───────────────────────────────
[Track record of each agent. Updated monthly or when
 predictions resolve. Feeds weighted voting.]

  | Agent     | Overall | Macro | Semi  | Geo   | Energy | Calls (n) |
  |-----------|---------|-------|-------|-------|--------|-----------|
  | @MACRO    | 0.72    | 0.81  | 0.45  | 0.55  | 0.60   | 34        |
  | @SEMI     | 0.75    | 0.50  | 0.88  | 0.40  | 0.35   | 28        |
  | @CHINA    | 0.68    | 0.60  | 0.55  | 0.78  | 0.50   | 31        |
  | @RISK     | 0.70    | 0.65  | 0.60  | 0.65  | 0.65   | 42        |
  [Scores = 1 - rolling Brier score, per domain]
  [Domain weight only applies when voting on that domain's call]

─── § ORGANIZATIONAL TRACK RECORD ──────────────────────
[Separate file: org_track_record.md. The group's history
 of collective decisions, validated over time.

 CREATION: Automatic. When a recommendation passes the
 group vote (status → APPROVED), system code creates an
 org track record entry linking back to the original
 agent recommendation.

 LIFECYCLE: Same as personal — code monitors, agent
 reflects at org level during evening meetings, context
 processor audits quarterly.]

ORG DECISION ENTRY SCHEMA (structured):
  # ── CREATED BY CODE WHEN REC IS APPROVED ──
  dec_id:           [auto: DEC-{structure}-{YYYYMMDD}-{seq}]
  source_rec_id:    [the REC-* that was approved]
  proposed_by:      [agent who made the recommendation]
  action:           BUY | SELL | HOLD | HEDGE
  asset:            [ticker]
  size_pct:         [approved size]
  group_confidence: [weighted vote result, float 0.0-1.0]
  vote_breakdown:   [{agent: confidence, weight, vote}...]
  opposed_by:       [{agent: reasoning}...]
  risk_assessment:  [risk agent's take at time of decision]
  entry_price:      [filled when order executes]
  target_price:     [from original rec]
  stop_price:       [from original rec]
  kill_condition:   [from original rec]
  created_at:       [timestamp]
  status:           PENDING

  # ── FILLED BY CODE ON RESOLUTION ──
  status:           PENDING → NEEDS_VALIDATION
  resolved_at:      [timestamp]
  resolution_trigger: [same enum as personal]
  exit_price:       [float]
  return_pct:       [float]
  market_return:    [SPX return over same period — benchmark]
  alpha:            [return_pct - market_return]

  # ── FILLED DURING EVENING MEETING REFLECTION ──
  status:           NEEDS_VALIDATION → VALIDATED
  outcome:          [same enum as personal]
  process_grade:    [A-F]
  outcome_grade:    [A-F]
  group_lesson:     [what the organization learned]
  attribution:      [who was right, who was wrong, and why]

  # ── FILLED BY CONTEXT PROCESSOR (quarterly audit) ──
  status:           VALIDATED → CONFIRMED
  audit_note:       [any discrepancies in self-assessment]

ORG SUMMARY (auto-computed by code):
  Total decisions: [n]
  Resolved: [n] | Pending: [n]
  Win rate: [%]
  Avg alpha: [% vs benchmark]
  Avg process grade: [letter]
  Calibration: [group confidence vs actual outcomes]

  Best collective call:  [dec_id + one-line summary]
  Worst collective call: [dec_id + one-line summary]

SYSTEMIC PATTERNS (context processor, quarterly):
  [Identified from aggregate decision data. Examples:
   - "We consistently enter too late — by the time consensus
     forms, 60% of the move has happened."
   - "Risk vetoes that were overridden: 3/4 were correct.
     We override risk too often."
   - "Cross-domain calls (agent A proposes, agent B from
     different domain validates) have 80% accuracy vs 62%
     for single-domain. Collaboration works."
   - "Overconfident on China macro: predicted 0.75 avg,
     actual accuracy 55%. @CHINA needs recalibration."

   These are the organizational Evolved Traits — how the
   group's collective intelligence changes over time.]

ATTRIBUTION TABLE (auto-computed by code):
  | Agent     | Proposed | Won | Lost | Win Rate | Avg Alpha | Avg Process |
  |-----------|----------|-----|------|----------|-----------|-------------|
  | @SEMI     | 12       | 9   | 3    | 75.0%    | +4.2%     | B+          |
  | @MACRO    | 8        | 4   | 4    | 50.0%    | +0.8%     | B           |
  | @GEO      | 5        | 3   | 2    | 60.0%    | +2.1%     | B           |
  [Not for punishment — for believability weight updates.]

═══════════════════════════════════════════════════════════
```

### Org Log Variations by Structure

**Structure A (Council):**
- Full log as shown above
- All agents visible in meeting summaries
- DM activity tracked separately (visible only to Risk agent)
- Weighted voting results shown with individual breakdowns

**Structure B (Firm):**
- Three sector-level logs (Tech&Supply, Macro&Rates, Geo&Resources)
  - Contains within-sector standup summaries
  - Sector head synthesis memos
- One CIO-level log
  - Contains Idea Dinner summaries
  - CIO allocation decisions with rationale
  - Only sees sector-level synthesis, NOT raw analyst output
- One Risk Committee log
  - Sees EVERYTHING from all levels
  - Contains risk reviews and veto decisions

**Structure C (Model):**
- No meeting summaries (no meetings)
- Daily signal log (all agent signals + optimizer output)
- Portfolio log (optimizer decisions + audit trail)
- Individual agent journals still maintained (for self-assessment)

---

## How These Documents Flow Through the Day

```
5:00 PM  WAKE
  │
  │  Agent reads: morning feed + yesterday's journal entry
  │
5:00-5:30  SOLO PRE-PREP
  │
  │  Agent writes: PRE-MEETING BRIEF (Document 1)
  │  Agent updates: journal watchlist, trigger checks
  │  ← Brief is LOCKED before meeting starts
  │
5:30-6:30  MORNING MEETING
  │
  │  Briefs are revealed simultaneously (write-first)
  │  Discussion of overnight events and recommendations
  │  System updates: org log morning meeting summary
  │
6:30-9:00  EXECUTION BLOCK
  │
  │  Agent works: deep research, DMs, private meetings
  │  Agent updates: journal (findings, thesis evidence,
  │    new triggers, prediction updates)
  │  System nudges: "Evening meeting in 60 min / 15 min"
  │
  │  Before evening meeting, agent writes:
  │  DAILY FINDINGS SUMMARY (short version of brief,
  │    focused on what was discovered during execution)
  │
9:00-10:00  EVENING MEETING
  │
  │  Findings summaries revealed simultaneously
  │  Discussion, debate, trade recommendations voted on
  │  System updates: org log evening meeting summary
  │
10:00-10:30  WIND-DOWN
  │
  │  Agent updates: journal daily log entry, self-assessment
  │  Agent places: conditional trade orders (if any)
  │  System updates: org log portfolio snapshot, orders
  │  Context processor: promotes journal → memory system
  │  Agent writes: next-wake sticky note
  │
  │  SLEEP
  │
6:00 AM   PRE-EXECUTION CHECK (code, no agents)
  │  Check pending orders against pre-market prices
  │  Execute / cancel / flag based on limits and kills
```

---

## "One Layer Deeper" Note

The pre-meeting brief recommendation section enforces a critical analytical
discipline from this project's reasoning framework. The chain must always be:

**Event → First-order → Second-order → Specific asset → Variant view → Trade**

This is the same principle as Bottleneck Descent (reasoning rule #2):
"When demand exceeds supply at Layer N, investigate Layer N-1." Applied to
communication, it means: never report a finding without tracing it to its
investment implication at least one layer below the obvious.

The Iran/Palantir example from reasoning-examples.md demonstrates this:
most analysts stop at "US strikes Iran → oil up." Our agents should reach
"→ intelligence was key → Palantir's Gotham platform → DoD contract catalyst
→ BUY PLTR" because that's where the alpha lives — in the layers the market
hasn't connected yet.

This should be added to `docs/knowledge/reasoning-examples.md` as an additional
worked example of the Bottleneck Descent / Depth-First Analysis rules applied
to real-time event analysis.

---

## Structured Decision Object Lifecycle

All agent decisions are structured objects with explicit fields — not natural
language prose. This is what makes them trackable. Here's how the objects
flow through the system:

```
AGENT PRODUCES STRUCTURED OUTPUT
═══════════════════════════════════════════════════════════

During pre-meeting brief or meeting:

  Agent emits REC-GEO-20260322-01:        Agent emits PRED-MACRO-20260322-01:
    action: BUY                              question: "Fed cuts ≥50bp
    asset: EPD                                          before Sep 2026?"
    confidence: 0.72                         probability: 0.32
    kill_condition: "Hormuz reopens"         resolution_date: 2026-09-01
    ...all other fields...                   ...all other fields...


SYSTEM CODE AUTOMATICALLY CREATES TRACK RECORD ENTRIES
═══════════════════════════════════════════════════════════

  REC-GEO-20260322-01                  PRED-MACRO-20260322-01
         │                                      │
         ▼                                      ▼
  CALL-GEO-20260322-01                CALL-MACRO-20260322-01
    source_type: REC                    source_type: PREDICTION
    statement: (copied verbatim)        statement: (copied verbatim)
    confidence: 0.72                    confidence: 0.32
    context_snapshot: {SPX, VIX...}     context_snapshot: {SPX, VIX...}
    status: PENDING                     status: PENDING


IF REC IS APPROVED BY GROUP → ORG TRACK RECORD ENTRY
═══════════════════════════════════════════════════════════

  REC-GEO-20260322-01 passes vote (0.68 weighted)
         │
         ▼
  DEC-COUNCIL-20260322-01
    source_rec_id: REC-GEO-20260322-01
    proposed_by: @GEOPOLITICS
    group_confidence: 0.68
    vote_breakdown: [{MACRO: 0.60, ...}, ...]
    status: PENDING
         │
         ▼
  ORDER-142 created (conditional limit order)
    asset: EPD, limit: $29.50, kill: $35
    status: QUEUED


CODE MONITORS DAILY
═══════════════════════════════════════════════════════════

  For each PENDING entry, code checks:

  ┌─ Price targets ──── market data API
  │  "EPD hit $36 target? No. EPD hit $26 stop? No."
  │
  ├─ Kill conditions ── data pipeline + keyword scan
  │  "Hormuz reopened? No."
  │
  ├─ Time expiry ────── calendar check
  │  "3-month timeframe expired? No."
  │
  └─ Events ─────────── news RSS keyword match
     "Iran diplomatic breakthrough? No."

  When ANY condition triggers:
    status: PENDING → NEEDS_VALIDATION
    outcome_data auto-filled by code


AGENT REFLECTS (next daily cycle)
═══════════════════════════════════════════════════════════

  System shows agent:
    "CALL-GEO-20260322-01 has resolved.
     You said BUY EPD at $29.50 with 0.72 confidence.
     EPD closed at $34.20 (+15.9%) when you recommended exit.
     Market return (SPX) over same period: +3.2%.
     Alpha: +12.7%."

  Agent fills:
    outcome: CORRECT
    what_right: "Hormuz risk premium was real and EPD was
                 the right vehicle — pipeline utilization
                 rose as predicted"
    what_wrong: "Underestimated timeline — thesis played
                 out in 6 weeks not 3 months"
    process_grade: A
    lesson: "Infrastructure plays on geopolitical risk
             resolve faster than expected because the
             risk premium prices in immediately even
             without actual disruption"


CONTEXT PROCESSOR AUDITS
═══════════════════════════════════════════════════════════

  Cheap LLM sees: original call + outcome + agent reflection

  Checks:
    - Agent graded self A. Outcome was +15.9% with sound
      reasoning. Grade is consistent. ✓
    - Lesson is specific and actionable. ✓
    - No inflated self-assessment detected.

  audit_grade: A
  audit_note: "Consistent. Lesson about timeline
               acceleration is novel — flag for
               identity.md promotion."
  status: CONFIRMED (permanent record)


CODE AGGREGATES (after any confirmation)
═══════════════════════════════════════════════════════════

  Personal track_record.md summary table recomputed:
    @GEOPOLITICS: 5 calls, 3 correct, 60% accuracy
    Geo domain: Brier 0.19 (good)
    Energy domain: Brier 0.22 (decent)

  Org org_track_record.md summary recomputed:
    Council total: 22 decisions, 14 wins, 63.6%
    @GEOPOLITICS attribution: proposed 5, won 3

  Believability weights updated:
    @GEOPOLITICS energy weight: 0.65 → 0.68


CONTEXT PROCESSOR PROMOTES (when significant)
═══════════════════════════════════════════════════════════

  Lesson "infrastructure plays resolve faster than expected"
  has now appeared in 2 separate calls.

  → Promoted to identity.md § Evolved Traits:
    "2026-05-15: Infrastructure plays on geopolitical risk
     resolve faster than expected. Adjusted timeframe
     estimates accordingly for future similar setups."
```

### What Lives Where (File Map)

```
agents/SEMI/
  identity.md        ← slow churn, who I am, evolved traits
  memory.md          ← moderate churn, relationships, knowledge
  recent.md          ← high churn, last session, open threads
  journal.md         ← daily operational log (theses, watchlist,
                       triggers, predictions, errors, self-assessment)
  track_record.md    ← append-only decision history, validated
                       over time. Structured entries only.

structures/council/
  org_log.md         ← daily meeting summaries, portfolio,
                       strategy, collective predictions
  org_track_record.md ← group decision history, validated.
                       Systemic patterns. Attribution.

structures/firm/
  sectors/tech_supply/sector_log.md
  sectors/macro_rates/sector_log.md
  sectors/geo_resources/sector_log.md
  cio_log.md
  risk_log.md
  org_track_record.md

structures/model/
  signal_log.md      ← all agent signals + optimizer output
  org_track_record.md
```

### Implementation Note

In the design phase, these are markdown schemas. In implementation, the
structured entries (REC-*, PRED-*, CALL-*, DEC-*) should live in a database
(SQLite for MVP, PostgreSQL for production). Reasons:

- Aggregation queries: "all SEMI calls on semiconductor stocks, last 6 months"
- Brier score computation across 100+ predictions
- Calibration table generation
- Cross-agent correlation analysis
- Believability weight recalculation

The markdown journal/log files become the HUMAN-READABLE VIEW generated from
the database, not the source of truth. The DB is the source of truth.
The context processor reads from DB, writes the markdown summaries that
agents see in their context window.
