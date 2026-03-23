# Analyst Journal & Decision-Tracking Frameworks

> Research compiled 2026-03-21. Sources: published interviews, books, academic research, institutional documentation, practitioner guides.
> Purpose: Inform the journal/memory schema design for AI analyst agents.

---

## Table of Contents

1. [Hedge Fund Analyst Notebooks](#1-hedge-fund-analyst-notebooks)
2. [George Soros's Reflexivity Journal](#2-george-soross-reflexivity-journal)
3. [Stanley Druckenmiller's Decision Framework](#3-stanley-druckenmillers-decision-framework)
4. [Ray Dalio's Decision Journal & Principles](#4-ray-dalios-decision-journal--principles)
5. [Michael Burry's Research Methodology](#5-michael-burrys-research-methodology)
6. [If-Then Contingency Planning](#6-if-then-contingency-planning)
7. [Prediction Tracking & Calibration Logs](#7-prediction-tracking--calibration-logs)
8. [Trading Journals](#8-trading-journals)
9. [Watchlists & Trigger Systems](#9-watchlists--trigger-systems)
10. [Long-Term Thesis Tracking](#10-long-term-thesis-tracking)
11. [Synthesis: Unified Agent Journal Schema](#11-synthesis-unified-agent-journal-schema)

---

## 1. Hedge Fund Analyst Notebooks

### Who Uses This

Fundamental long/short equity analysts at multi-manager platforms (Point72, Citadel, Millennium), single-manager funds (Pershing Square, Greenlight), and sell-side research desks that feed them.

### The Investment Memo Format

The industry-standard unit of analyst work product is the **investment memo** (also called a "stock pitch" or "write-up"). At Point72 Academy, analysts are trained to produce these over a 10-week bootcamp covering accounting, modeling, data analysis, and compliance before being assigned to a PM team.

#### Standard Sections

```
INVESTMENT MEMO
===============
1. RECOMMENDATION
   - Direction: Long / Short
   - Current price, market cap
   - Target price, time horizon
   - Expected return (base / bull / bear)

2. VARIANT VIEW
   - What the market consensus is
   - What the market has WRONG (the edge)
   - Why the mispricing exists (behavioral, structural, informational)

3. THESIS (1-3 sentences)
   - What the business is
   - Why it is mispriced
   - What catalyst closes the gap

4. BUSINESS ANALYSIS
   - Industry structure, competitive dynamics
   - Unit economics, business model
   - Management quality and incentive alignment

5. FINANCIAL MODEL
   - Income statement / EBITDA by segment
   - Cash flow analysis (FCF, capex, working capital)
   - Balance sheet / leverage (debt paydown, ratios)

6. VALUATION
   - DCF, comps, precedent transactions
   - Base / bull / bear scenario with explicit assumptions
   - Each scenario: target price, % return, probability weight

7. CATALYST & TIMELINE
   - What specific event will re-rate the stock
   - When it should occur
   - What happens if catalyst doesn't materialize

8. RISKS & KILL CONDITIONS
   - Top 3-5 risks ranked by probability and impact
   - Explicit conditions under which the thesis is DEAD
   - Pre-committed exit triggers (price, fundamental, time)

9. POSITION SIZING RATIONALE
   - Conviction level (1-5 or percentage)
   - Correlation to existing portfolio positions
   - Max drawdown tolerance
```

#### Key Point72 Training Details

- First 5 weeks: classroom (accounting, statistics, economics, modeling, SQL, data science, alternative data)
- Weeks 6-10: assigned to a PM team, live research, final stock pitch
- Pitch format: 10+ page written document with 3-statement model
- Must demonstrate top-down AND bottom-up analysis
- Clear investment thesis with risk assessment and defined catalysts
- Deep sector specialization beats generalist coverage (one expert TMT analyst > three generalists)

### Why It Works

- Forces completeness: you cannot submit a memo without addressing risks and kill conditions
- Variant view requirement forces you to articulate your EDGE, not just your opinion
- Scenario analysis with probability weights makes conviction explicit and auditable
- Kill conditions pre-commit you to exits, reducing emotional attachment
- Standardized format enables comparison across analysts and over time

### Translation to AI Agent Journal

- Each investment thesis gets a structured memo entry in the agent's memory
- Kill conditions become automated monitoring triggers
- Variant view becomes a required field that prevents "consensus restating"
- Conviction level (1-5) feeds directly into believability-weighted aggregation across agents
- Catalyst timeline creates automatic revisit dates

---

## 2. George Soros's Reflexivity Journal

### Source

*The Alchemy of Finance* (1987), specifically Part III: "The Real-Time Experiment" -- a live diary of Soros's trading decisions from August 1985 to November 1986.

### What He Actually Wrote

Soros kept a real-time diary of his investment thinking, recording his trades in currencies, commodities, bonds, and stock indexes. The experiment was organized into distinct phases:

```
PHASE STRUCTURE
===============
- The Starting Point (August 1985)
- Phase 1 (August 1985 - December 1985) -- successful
- Control Period (January 1986 - July 1986)
- Phase 2 (July 1986 - November 1986) -- losses
- The Conclusion (November 1986)
```

#### Diary Entry Structure (reconstructed from the book)

Each entry typically contained:

```
SOROS DIARY ENTRY
=================
1. CURRENT MACRO THESIS
   - The prevailing reflexive process he sees playing out
   - Which feedback loop (cognitive vs. manipulative) is dominant
   - Where in the boom/bust cycle the market sits

2. POSITION INVENTORY
   - All current positions across asset classes
   - Size and direction of each
   - How they relate to the macro thesis

3. SCENARIO THINKING
   - What could happen next (multiple scenarios)
   - How each scenario would affect his positions
   - What he would do in response to each scenario

4. ACTIONS TAKEN
   - What he actually traded and why
   - How the new positions change his portfolio's exposure
   - Whether he is adding to conviction or hedging

5. RETROSPECTIVE
   - What happened vs. what he expected
   - Whether the reflexive process is proceeding as theorized
   - Where his model was wrong
```

### The Pain Signal

Soros famously used physical discomfort as an early warning system:

- When he was actively running his fund, he suffered from chronic backache
- He used the onset of acute pain as a signal that something was wrong in his portfolio
- His son Robert Soros: "The reason he changes his position on the market or whatever is because his back starts killing him. It has nothing to do with reason. He literally goes into a spasm, and it's this early warning sign."
- The backache did NOT tell him what was wrong -- it simply prompted him to look for something amiss
- This maps to the neuroscience concept of **somatic markers** (Damasio): physical sensations that mark important information, carried through the body rather than through explicit reasoning
- Soros himself called it "animal instincts" -- pattern recognition honed over decades manifesting as bodily signals

### The Reflexivity Framework Embedded in the Journal

Every diary entry was structured around his theory of reflexivity:

```
REFLEXIVITY CYCLE (Soros)
=========================
1. Identify the prevailing BIAS (market narrative)
2. Identify the prevailing TREND (price action)
3. Assess feedback: Is the bias reinforcing the trend?
   - If yes: boom phase, ride it
   - If reflexive feedback is breaking down: bust approaching
4. Look for the INFLECTION POINT where bias and trend diverge
5. Position for the reversal
```

### Why It Works

- Writing forced disciplined thinking: Soros said the diary made his arguments "more consistent than they would have been" without writing
- The real-time format prevented hindsight bias -- he could not retroactively edit his reasoning
- Scenario thinking before events occur creates a decision tree that speeds reaction time
- Explicit position inventory keeps the whole portfolio visible, preventing "forgotten" exposure
- Physical pain tracking (however unconventional) demonstrates the value of monitoring non-rational signals

### Translation to AI Agent Journal

- Agents should maintain a rolling macro thesis document that gets updated (not overwritten) with timestamps
- Position inventory should be a first-class section, always visible
- Scenario thinking should be structured: for each thesis, enumerate 2-3 scenarios with conditional actions
- Reflexivity cycle maps directly to the project's "Crack Detection" reasoning rule
- The "pain signal" equivalent for an AI agent: anomaly detection on its own prediction accuracy -- when its recent predictions start diverging from outcomes, flag it as a meta-signal that the model of the world needs revision

---

## 3. Stanley Druckenmiller's Decision Framework

### Source

Interviews, speeches, and analysis of his 30-year track record (30% annualized returns, no losing year at Duquesne Capital).

### Core Framework

Druckenmiller does not use a formal written journal in the traditional sense. His framework is a mental operating system with explicit rules:

```
DRUCKENMILLER DECISION FRAMEWORK
=================================
1. LIQUIDITY IS KING
   "Earnings don't move the overall market; it's the Federal
   Reserve Board... focus on the central banks and focus on
   the movement of liquidity... it's liquidity that moves markets."

   Primary monitor: Central bank policy direction
   Secondary: Credit conditions, money supply, yield curve

2. LEADING INDICATORS (his macro dashboard)
   - Housing starts / sales
   - Auto sales
   - Durable goods / big-ticket consumption
   (All three share sensitivity to interest rates)
   - Inflation trends
   - Employment data
   - Policy inflection points (government spending, regulation)

3. CONVICTION-BASED SIZING
   "Sizing is 70% to 80% of the equation."
   "It's not whether you're right or wrong, it's how much you
   make when you're right and how much you lose when you're wrong."

   Conviction scale (implicit):
   - Low conviction: small or no position
   - Medium conviction: standard position
   - High conviction: concentrated bet ("go for the jugular")
   - Maximum conviction: bet the farm (e.g., breaking the Bank
     of England -- $10B+ short on sterling)

4. THE STREAK FACTOR
   "I believe in streaks. Like in baseball. Sometimes you're
   seeing the ball, sometimes you're not. My number one job is
   to know when I'm hot and when I'm not. When I'm hot, I need
   to turn the dial straight up. When you're cold the last thing
   you should do is make big bets to get even."

5. FAST EXIT ON THESIS BREAK
   - Exits quickly when data changes or assumptions break
   - No ego attachment to positions
   - Flexibility across asset classes (stocks, bonds, currencies,
     commodities -- whatever the thesis demands)

6. ASYMMETRY FILTER
   "Liquidity, policy inflection points, and the asymmetry
   between upside and downside drive timing."
   - Only take trades where upside significantly exceeds downside
   - The asymmetry must be structural, not hopeful
```

### What He Monitors (Macro Dashboard)

```
DRUCKENMILLER MONITORING CHECKLIST
====================================
Central Banks:
  [ ] Fed policy direction (hawkish/dovish shift)
  [ ] ECB, BOJ, PBOC policy divergence
  [ ] Liquidity injections or withdrawals
  [ ] Forward guidance changes

Leading Economic Indicators:
  [ ] Housing starts and home sales
  [ ] Auto sales trends
  [ ] Durable goods orders
  [ ] Consumer credit growth
  [ ] Manufacturing PMI
  [ ] Employment (initial claims, NFP trends)

Market Signals:
  [ ] Yield curve shape and movement
  [ ] Credit spreads (IG and HY)
  [ ] Currency cross-rates (especially USD)
  [ ] Commodity price trends (oil, copper)

Policy & Fiscal:
  [ ] Government spending trajectory
  [ ] Regulatory changes
  [ ] Tax policy shifts
  [ ] Geopolitical inflection points

Self-Assessment:
  [ ] Current P&L trajectory (hot vs. cold streak)
  [ ] Recent prediction accuracy
  [ ] Emotional state and cognitive clarity
```

### Why It Works

- Liquidity-first hierarchy prevents getting lost in micro-level noise
- Conviction-based sizing ensures capital is allocated to highest-quality ideas
- The "streak" self-assessment adds a meta-cognitive layer most frameworks lack
- Fast exits prevent small losses from becoming catastrophic
- Asymmetry filter ensures positive expected value even with imperfect timing

### Translation to AI Agent Journal

- Each agent should maintain a macro dashboard (subset relevant to their domain)
- Conviction levels should be explicit numbers (1-5) that directly weight position sizing recommendations
- Agents should track their own recent accuracy (equivalent of "hot/cold streak")
- Every position recommendation must include an asymmetry assessment: upside vs. downside ratio
- Fast exit capability means kill conditions must be monitored automatically, not just recorded

---

## 4. Ray Dalio's Decision Journal & Principles

### Source

*Principles* (2017), Bridgewater Associates institutional tools, published essays.

### The Three-Tool System

Dalio uses three interconnected tools for decision tracking:

#### Tool 1: The Error Log / Issue Log

```
BRIDGEWATER ISSUE LOG ENTRY
============================
Date:
Who is responsible:
Severity: [1-5]

What went wrong:
  - Factual description of the error or bad outcome

Root cause:
  - Why did this happen? (not surface-level)
  - What principle was violated?
  - Was it a knowledge gap, process failure, or judgment error?

Lesson / New Principle:
  - What decision rule should prevent this in the future?
  - Does an existing principle need modification?

Follow-up:
  - [ ] Principle documented
  - [ ] Communicated to relevant parties
  - [ ] Process change implemented
```

The cardinal rule: **If something went badly and you logged it, you were okay. If you didn't log it, you were in deep trouble.** The log itself was not punitive -- it was a learning system.

#### Tool 2: The Decision Criteria Journal

Dalio's personal practice of writing down decision-making criteria:

```
DALIO DECISION CRITERIA ENTRY
==============================
Decision:
  - What decision was made

Context:
  - What information was available at the time

Criteria Used:
  - What rules/principles guided the decision
  - What weights were given to different factors
  - What trade-offs were explicitly considered

Expected Outcome:
  - What I expect to happen and why
  - Timeframe for evaluation

Actual Outcome (filled in later):
  - What actually happened
  - Delta from expectation

Reflection:
  - Were the criteria correct?
  - Should the criteria be modified?
  - New principle extracted (if any)
```

Over time, Dalio's collection of these entries became his *Principles* -- "a collection of recipes for decision making."

#### Tool 3: The Pain Button

A Bridgewater app for tracking emotional/somatic responses:

```
PAIN BUTTON LOG
===============
Timestamp:
Emotion: [anger | disappointment | frustration | worry | rejection | other]
Intensity: [1-10]
Context: What triggered this feeling

(Later reflection -- guided questions):
  - Was this pain productive (pointing to something real)?
  - What action did I take in response?
  - Was that action productive?
  - What would I do differently next time?
```

The app tracks frequency of pain episodes, their causes, and whether subsequent actions were productive. Dalio describes it as "like having a psychologist in your pocket."

#### The Believability-Weighted System (Institutional, not personal)

For group decisions at Bridgewater:

```
BRIDGEWATER DECISION TOOLS
===========================
Baseball Cards:
  - Every employee has a "card" showing strengths/weaknesses
  - Evidence-based: compiled from reviews, tests, tracked decisions
  - Updated continuously as new data comes in

Dot Collector:
  - Real-time meeting tool
  - Attendees rate each speaker on relevant traits (1-10) as they talk
  - System shows both equal-weighted and believability-weighted averages
  - If the two averages align: matter resolved
  - If they diverge: dig deeper, default to believability-weighted

Believability Weighting Criteria:
  1. Has repeatedly and successfully accomplished the thing in question
  2. Can logically explain cause-effect behind their conclusions
```

#### The Daily Observations

Bridgewater's institutional journal -- published for 50+ years:

- A daily white paper ("the wire") read by central bankers and pension fund managers worldwide
- Gives real-time transparency into how Bridgewater's thinking is evolving
- Systematic approach: identify underlying causes, anticipate market movements
- Hundreds of codified "decision rules" are incorporated into computer analysis
- All meetings are filmed; every decision conversation is part of the institutional record

### Why It Works

- Error log creates a blame-free learning culture (the punishment is for NOT logging, not for errors)
- Decision criteria journal separates the decision from the outcome -- you can make a good decision with a bad outcome and vice versa
- Pain Button captures emotional data that pure analytical frameworks miss
- Believability weighting prevents loudest-voice-wins dynamics
- Daily Observations create an auditable institutional memory spanning decades

### Translation to AI Agent Journal

- Every agent should maintain an error log: when predictions are wrong, force structured reflection
- Decision criteria should be explicit and auditable -- the agent should record WHY it made a call, not just WHAT call it made
- The Pain Button maps to an "uncertainty flag" -- when an agent's confidence is low or conflicting signals appear, log it explicitly
- Believability weighting translates directly to the project's track-record-based influence system
- Daily observations format is the model for each agent's `recent.md` wake-up briefing

---

## 5. Michael Burry's Research Methodology

### Source

*The Big Short* (Lewis, 2010), Burry's Silicon Investor posts (1996-2000), Scion Capital letters, SEC filings, published interviews.

### The Research Process

Burry's approach is obsessive primary-source analysis with zero reliance on consensus or third-party opinions.

```
BURRY RESEARCH METHODOLOGY
============================
STEP 1: PRIMARY SOURCE IMMERSION
  - Read every SEC filing (10-K, 10-Q, proxy statements, 8-K)
  - Focus on FOOTNOTES, not headlines
  - Read prospectuses and legal documents in full
  - For the Big Short: read individual mortgage bond prospectuses,
    examining loan-level data, underwriting standards, borrower
    credit profiles, prevalence of adjustable-rate and
    interest-only mortgages

STEP 2: QUANTITATIVE SCREENING
  - Compare capital expenditures to cash flows (last 10 years)
  - Earnings consistency and growth trajectory
  - Free cash flow (FCF) as the primary health indicator
  - Enterprise Value / EBITDA ratio
  - Margin of safety calculation (Graham-Dodd framework)

STEP 3: ENTRY CRITERIA
  - Prefer buying within 10-15% of 52-week low with price support
  - Bare-bones technical analysis only for entry timing
  - Cut losses when stocks break to new lows
  - 100% based on margin of safety concept

STEP 4: DOCUMENTATION
  - Extensive personal notes on each company's fundamentals
  - Public posts on Silicon Investor (3,304 posts, 1996-2000)
  - Detailed write-ups shared with investment community
  - Scion Capital investor letters documenting thesis evolution
```

### Silicon Investor Era (1996-2000)

Before starting Scion Capital, Burry documented his entire investment process publicly:

- Founded 25+ message boards on Silicon Investor including "Value Investing" and "Buffetology"
- Averaged 2+ posts per day for 4 years
- Focused on stocks in or smaller than the S&P Midcap 400
- Each analysis followed a consistent structure:
  - Company description and business model
  - Key financial metrics (FCF, EV/EBITDA, earnings trajectory)
  - Price context (52-week range, support levels)
  - Margin of safety calculation
  - Risks and what could go wrong
  - Why the market was wrong (the edge)

### The Big Short Research (2004-2007)

The mortgage analysis that led to the greatest trade in history:

```
BURRY'S MORTGAGE RESEARCH PROCESS
====================================
1. Bottom-up loan-level analysis
   - Individual mortgage bond prospectuses
   - Underwriting standards per pool
   - Borrower credit profiles
   - ARM and interest-only mortgage prevalence
   - Teaser rate expiration schedules

2. What he found that others missed:
   - Rating agencies weren't reading the prospectuses
   - Underwriting standards had collapsed
   - Teaser rates on ARMs would reset 2006-2007
   - Borrowers couldn't afford reset payments
   - The entire structure was built on home prices never falling

3. How he organized the research:
   - Spreadsheet tracking every mortgage pool he analyzed
   - Timeline of when ARM resets would hit
   - Tracking which tranches to short via CDS
   - Cross-referencing loan-to-value ratios with housing price data
```

### Why It Works

- Primary source analysis creates information asymmetry -- Burry read documents that analysts, rating agencies, and investors were supposed to read but didn't
- Quantitative screening before deep-dive prevents wasting time on low-quality candidates
- Public documentation (Silicon Investor) created accountability and forced clear thinking
- The obsessive footnote-reading habit catches things that surface-level analysis misses (financial engineering, off-balance-sheet liabilities, related-party transactions)
- Margin of safety provides a built-in error buffer for inevitable misjudgments

### Translation to AI Agent Journal

- AI agents should have a "research depth" indicator -- are they working from summaries or primary sources?
- Footnote analysis is the AI equivalent of reading full SEC filings rather than just relying on financial data APIs
- The margin of safety concept translates to requiring a minimum expected-value threshold before recommending any position
- Public documentation (the Silicon Investor pattern) maps to agents publishing their reasoning in shared channels before decisions
- Timeline tracking (when ARM resets hit) maps to the if-then trigger system

---

## 6. If-Then Contingency Planning

### Military Decision Support Matrix (DSM)

The most rigorous if-then framework comes from military doctrine, specifically U.S. Army planning processes.

```
MILITARY DECISION SUPPORT MATRIX
==================================
Decision   | Trigger       | Conditions     | Action if     | Action if
Point      | (NAI/Event)   | Required       | YES           | NO
-----------|---------------|----------------|---------------|----------
DP-1       | Enemy crosses | Recon confirms | Execute       | Continue
           | Phase Line X  | 2+ battalion   | Branch Plan A | main plan
           |               | strength       |               |
-----------|---------------|----------------|---------------|----------
DP-2       | Supply route  | Logistics      | Switch to     | Maintain
           | interdicted   | officer        | alternate     | current
           |               | confirms       | route (PACE)  | supply
           |               |                |               | route
```

Key concepts:
- **Decision Point (DP):** The LAST possible moment at which a decision can be made for an action to occur in time
- **Named Area of Interest (NAI):** What to watch (the observable indicator)
- **Target Area of Interest (TAI):** Where to act if triggered
- **Branch Plan:** Pre-developed alternative course of action
- **PACE framework:** Primary, Alternate, Contingency, Emergency plans

### CIA Structured Analytic Techniques

#### Key Assumptions Check (KAC)

```
KEY ASSUMPTIONS CHECK
======================
Assumption | Evidence     | Confidence | What if    | Impact if
           | Supporting   | Level      | WRONG?     | Wrong
-----------|-------------|------------|------------|----------
China will | Trade data, | Medium     | Retaliatory| Short-term
not        | diplomat    |            | tariffs on | pain for
retaliate  | statements  |            | US ag,     | US farmers,
on tariffs |             |            | tech       | risk
           |             |            |            | escalation
```

The KAC is recommended for ALL major analytical projects at CIA. Purpose: make explicit the assumptions that guide interpretation of evidence. Most useful early in the assessment process.

#### Analysis of Competing Hypotheses (ACH)

```
ACH MATRIX
============
Evidence /     | H1: China    | H2: China   | H3: China
Argument       | cooperates   | retaliates  | decouples
               |              | targeted    | fully
---------------|-------------|-------------|----------
Trade surplus  | Consistent  | Consistent  | Inconsistent
shrinking      |             |             |
---------------|-------------|-------------|----------
Military       | Inconsistent| Consistent  | Consistent
buildup in SCS |             |             |
---------------|-------------|-------------|----------
Tech self-     | Inconsistent| Neutral     | Consistent
sufficiency    |             |             |
push           |             |             |
```

Key principle: **Focus on DISPROVING hypotheses, not proving your preferred one.** Eliminate least consistent hypotheses through evidence accumulation.

### Gary Klein's Pre-Mortem

```
PRE-MORTEM FRAMEWORK
=====================
1. BRIEFING
   - State the decision/project and its goals

2. ASSUME FAILURE
   "Imagine we are 6 months in the future. This trade has
   failed catastrophically. Why?"

3. INDEPENDENT GENERATION (each team member separately)
   - List all plausible reasons for failure
   - No filtering, no judgment at this stage

4. CONSOLIDATION
   - Merge and deduplicate failure reasons
   - Rank by probability and impact

5. MITIGATION
   - For each top failure mode:
     - What trigger/indicator would we see FIRST?
     - What action would we take?
     - Can we structure the trade to limit this risk?

6. KILL CONDITIONS (output)
   - Concrete, measurable conditions that would invalidate thesis
   - Pre-committed actions for each
```

### Translating to Investment If-Then Framework

```
INVESTMENT IF-THEN TRIGGER
============================
Trigger ID: MACRO-2026-003
Domain: Macro / Employment

IF:
  - US unemployment rate crosses 5.0% (currently 3.8%)
  - AND initial claims trend exceeds 300K for 3 consecutive weeks

THEN:
  - Reduce equity exposure by 20%
  - Increase duration (long bonds)
  - Review all cyclical positions for exit

CONFIDENCE: 0.75 that this signal precedes recession by 6-9 months

MONITORING:
  - Data source: BLS monthly release, DOL weekly claims
  - Check frequency: Weekly (Thursday claims), Monthly (NFP)
  - Agent responsible: Macro Analyst

EXPIRY: 2027-03-01 (reassess if conditions change materially)

LAST REVIEWED: 2026-03-21
```

### Why It Works

- Military DSM forces pre-thinking of decision points before they arrive (no ad-hoc scrambling)
- KAC makes hidden assumptions visible and testable
- ACH prevents confirmation bias by requiring evidence evaluation against ALL hypotheses
- Pre-mortem harnesses "prospective hindsight" -- people generate 30% more failure reasons when imagining it already happened
- Investment if-then triggers automate the connection between monitoring and action

### Translation to AI Agent Journal

- Each agent should maintain a set of if-then triggers relevant to their domain
- Triggers should have explicit monitoring sources, check frequencies, and expiry dates
- The KAC format maps directly to tracking assumptions underlying each investment thesis
- ACH matrix is the structure for the agent's "Counterparty Test" (reasoning rule #4)
- Pre-mortem should run automatically on every new thesis before it enters the portfolio
- Decision points should have deadlines -- the concept of "last possible moment to decide" prevents perpetual monitoring without action

---

## 7. Prediction Tracking & Calibration Logs

### Source

Philip Tetlock's Good Judgment Project (GJP), Metaculus forecasting platform, prediction markets research.

### Superforecaster Prediction Record Format

```
PREDICTION RECORD
==================
ID: PRED-2026-047
Date Created: 2026-03-15
Last Updated: 2026-03-21

QUESTION:
  "Will the Federal Reserve cut rates by 50+ bps before
  September 2026?"

PROBABILITY ESTIMATE: 0.35

REASONING:
  - Inflation still above 3% target (against)
  - Labor market showing early weakness (for)
  - Fed rhetoric still hawkish (against)
  - Historical pattern: Fed usually late to cut (for)
  - Weighting: inflation data dominates until trend reversal

KEY ASSUMPTIONS:
  - No major financial crisis forcing emergency cut
  - CPI continues gradual decline trajectory
  - No geopolitical shock to energy prices

UPDATE LOG:
  | Date       | New P | Delta  | Reason                    |
  |------------|-------|--------|---------------------------|
  | 2026-03-15 | 0.35  | --     | Initial estimate          |
  | 2026-03-21 | 0.32  | -0.03  | Strong jobs report        |
  | ...        |       |        |                           |

RESOLUTION:
  - Resolution date: 2026-09-01
  - Outcome: [pending]
  - Brier score: [pending]

TAGS: [macro] [fed] [rates] [high-impact]
```

### Calibration Tracking (Metaculus / GJP model)

```
CALIBRATION LOG (aggregated across predictions)
=================================================
Bucket     | # Predictions | # Resolved YES | Actual Rate | Delta
-----------|--------------|----------------|-------------|------
0-10%      | 23           | 1              | 4.3%        | OK
10-20%     | 31           | 5              | 16.1%       | OK
20-30%     | 18           | 4              | 22.2%       | OK
30-40%     | 25           | 11             | 44.0%       | +9%
40-50%     | 22           | 8              | 36.4%       | -9%
50-60%     | 19           | 11             | 57.9%       | OK
60-70%     | 28           | 21             | 75.0%       | +10%
70-80%     | 34           | 26             | 76.5%       | OK
80-90%     | 27           | 24             | 88.9%       | OK
90-100%    | 15           | 15             | 100.0%      | +5%

Overall Brier Score: 0.189
Calibration: Good (slight overconfidence in 60-70% range)
Resolution: Above average
```

### Superforecaster Best Practices (from GJP research)

1. **Update frequently but in small increments** -- top superforecasters averaged 16+ updates per question per year
2. **Granularity predicts accuracy** -- precise estimates (e.g., 0.73 vs. "about 70%") correlate with better calibration
3. **Track what reasoning method you used** -- GJP asked forecasters to record which training techniques they applied
4. **Decompose complex questions** -- break "Will X happen?" into component probabilities and combine
5. **Score yourself ruthlessly** -- Brier score is the gold standard (0 = perfect, 1 = always wrong, 0.25 = coin flip)

### Brier Score Calculation

```
Brier Score = (forecast - outcome)^2

For a yes/no question where you said 70% and it happened:
  Score = (1.0 - 0.7)^2 + (0.0 - 0.3)^2 = 0.09 + 0.09 = 0.18

Interpretation:
  0.00 = Perfect (you always said 100% for things that happened)
  0.25 = Coin flip baseline (no skill)
  0.50 = Terrible (worse than random)

Top superforecasters: ~0.15 (30-60% better than average participants)
```

### Why It Works

- Probability format forces precision -- "likely" is meaningless; "0.72" is auditable
- Update logs create accountability for belief revision (or failure to revise)
- Calibration tracking across many predictions reveals systematic biases (overconfidence, underconfidence)
- Brier scoring provides a single number for comparing forecaster quality
- Tagging enables domain-specific calibration analysis (you might be well-calibrated on macro but overconfident on geopolitics)

### Translation to AI Agent Journal

- Every directional call should include a probability estimate
- Probability estimates should be updated with timestamps and reasons (not overwritten)
- Agents should maintain a running calibration log, updated as predictions resolve
- Brier score becomes a core input to believability weighting across agents
- Domain-specific calibration scores determine which agents get higher weight on which types of questions

---

## 8. Trading Journals

### Source

Professional trading desks, prop shops, and trading psychology research.

### Professional Trading Journal Format

```
TRADE JOURNAL ENTRY
=====================
Trade ID: T-2026-0142
Date/Time: 2026-03-21 09:32 EST
Session: New York open

INSTRUMENT: NVDA (Nvidia Corp)
DIRECTION: Long
ENTRY PRICE: $142.50
POSITION SIZE: 200 shares ($28,500)
RISK: 2.1% of portfolio

SETUP / THESIS:
  - Type: Earnings momentum + sector rotation
  - Signal: Breakout above 50-day MA on 2x avg volume
  - Macro context: Semi cycle upturn confirmed by TSMC guidance
  - Edge: Market underpricing datacenter demand growth rate

ENTRY CRITERIA MET:
  [x] Price above 50-day MA
  [x] Volume confirmation (2x+ average)
  [x] Sector ETF (SMH) confirming
  [x] No conflicting macro signals
  [ ] Insider buying (not present, but not required)

EXIT PLAN:
  - Target 1: $155 (partial -- 50% of position)
  - Target 2: $170 (remainder)
  - Stop loss: $135 (5.3% risk)
  - Time stop: Exit if thesis not confirmed within 4 weeks
  - Thesis kill: Exit if TSMC revises guidance down

RISK/REWARD:
  - Upside to T1: +8.8% ($2,500)
  - Upside to T2: +19.3% ($5,500)
  - Downside to stop: -5.3% (-$1,500)
  - R:R ratio: 1.7:1 (T1) / 3.7:1 (T2)

EMOTIONAL STATE:
  Pre-trade: Calm, methodical [7/10 confidence]
  During: [filled in real-time]
  Post-trade: [filled after exit]

MARKET CONTEXT:
  - VIX: 18.5 (moderate)
  - SPX: +0.3% at entry
  - Sector: Semis +1.2%
  - News: None material

POST-TRADE ANALYSIS (filled after exit):
  Exit date:
  Exit price:
  P&L:
  What went right:
  What went wrong:
  Lesson:
  Grade (A-F):
```

### Advanced Fields for Options/Derivatives

```
OPTIONS-SPECIFIC FIELDS
========================
Strategy: [call spread / put spread / iron condor / etc.]
Expiration: [date]
Strike(s): [price(s)]
IV at entry: [%]
IV at exit: [%]
Greeks at entry:
  - Delta:
  - Theta:
  - Vega:
  - Gamma:
Time decay impact:
Volatility impact:
```

### Key Data Points Tracked (Industry Standard)

1. **Date and time** -- enables session-specific analysis
2. **Instrument and direction** -- what and which way
3. **Entry and exit prices** -- the hard numbers
4. **Position size and risk** -- how much skin in the game
5. **Setup/thesis** -- why you took the trade (the most important field)
6. **Screenshots** -- chart at entry and exit (visual record)
7. **Emotional state** -- how you felt before, during, and after
8. **Market context** -- what the broader market was doing
9. **Post-trade analysis** -- what worked, what didn't, grade yourself
10. **Lesson** -- one concrete takeaway

### Review Cadence

- **Daily:** Quick scan of open positions, check stops
- **Weekly:** Review all trades from the week, pattern analysis
- **Monthly:** Statistical review (win rate, avg win/loss, Sharpe ratio, max drawdown)
- **Quarterly:** Strategy-level review (which setups work? which don't?)

### Why It Works

- The thesis field is the anchor -- if you can't articulate WHY, it's gambling
- Emotional tracking reveals patterns (e.g., overtrading when anxious, revenge trading after losses)
- Risk/reward pre-calculation prevents entering asymmetrically bad trades
- Post-trade grading separates process from outcome (an A-grade trade can lose money)
- Weekly/monthly reviews convert individual data points into pattern recognition

### Translation to AI Agent Journal

- Each trade recommendation should carry a structured entry matching this format
- AI agents don't have emotions, but they DO have confidence levels and conflicting signals -- track those
- The "setup type" taxonomy is important: agents should categorize their trades for pattern analysis
- Post-trade analysis is critical for agent learning -- force structured reflection on every closed position
- The grade field (A-F) maps to the agent's self-assessment of decision quality vs. luck

---

## 9. Watchlists & Trigger Systems

### The Three-Tier Watchlist Model

Professional analysts maintain watchlists at three levels of attention:

```
TIER 1: ACTIVE POSITIONS (Daily monitoring)
=============================================
These are positions in the portfolio. Maximum attention.

Per item:
  - Ticker, direction, entry date, entry price
  - Current P&L
  - Kill conditions (list, with checkboxes)
  - Next catalyst date
  - Position size as % of portfolio
  - Correlation flag (what else moves if this moves)

TIER 2: READY LIST (Weekly monitoring)
========================================
Fully researched names waiting for entry trigger.

Per item:
  - Ticker
  - Full thesis (link to memo)
  - Entry trigger: specific condition that must be met
  - Target entry price range
  - Position size plan
  - Why waiting (what's missing from the setup)
  - Expiry: date by which if trigger hasn't fired, reassess

TIER 3: RESEARCH PIPELINE (Monthly monitoring)
===============================================
Names under investigation but not yet fully researched.

Per item:
  - Ticker or theme
  - Initial observation (what caught attention)
  - Key questions to answer
  - Research status [screening / deep-dive / modeling / ready]
  - Next step and deadline
  - Priority level [high / medium / low]
```

### Conditional Trigger Format

```
TRIGGER ENTRY
==============
ID: TRIG-2026-018
Created: 2026-03-15
Domain: Semiconductor supply chain

WATCHED VARIABLE:
  TSMC monthly revenue data (released ~10th of each month)

TRIGGER CONDITION:
  IF TSMC monthly revenue growth declines for 3 consecutive months
  AND SOXX index has not yet corrected >10% from peak

THEN:
  1. Initiate short position in overvalued semi names (list: ASML, KLAC)
  2. Alert Council / CIO (depending on structure)
  3. Review all long semi positions for exit

RATIONALE:
  TSMC revenue is the single best leading indicator of semiconductor
  cycle turns. Market typically lags this signal by 2-3 months.

MONITORING:
  Source: TSMC IR website, released monthly
  Frequency: Monthly check, ~10th of each month
  Agent: Semi Supply Chain Analyst

STATUS: [armed | triggered | expired | cancelled]
LAST CHECKED: 2026-03-10 (Feb data showed +12% YoY, no trigger)
EXPIRY: 2027-03-15
```

### Event-Driven Trigger Taxonomy

```
TRIGGER TYPES
==============
1. THRESHOLD TRIGGERS
   "If X crosses Y level..."
   Examples: unemployment > 5%, VIX > 30, 10Y yield > 5%

2. TREND TRIGGERS
   "If X does Y for N consecutive periods..."
   Examples: 3 months declining PMI, 4 weeks rising claims

3. EVENT TRIGGERS
   "If specific event occurs..."
   Examples: Fed emergency meeting, TSMC fab disruption, China Taiwan action

4. DIVERGENCE TRIGGERS
   "If X and Y diverge beyond Z..."
   Examples: credit spreads widen while equities rally (bearish divergence)

5. COMPOSITE TRIGGERS
   "If A AND B AND NOT C..."
   Examples: unemployment rising AND yield curve inverted AND Fed still hawkish

6. TIME TRIGGERS
   "On date X, reassess Y..."
   Examples: quarterly earnings review, annual thesis refresh, options expiry
```

### Why It Works

- Three tiers prevent "everything is equally important" syndrome
- Explicit entry triggers on the Ready List prevent premature entry or missed opportunities
- Trigger expiry dates prevent zombie watchlist items that accumulate forever
- Conditional triggers automate the if-then connection between data and action
- Trigger taxonomy provides a common language for different types of signals

### Translation to AI Agent Journal

- Each agent maintains their own domain-specific watchlist across all three tiers
- Triggers should be machine-parseable (structured fields, not prose)
- The monitoring section should map directly to data pipeline subscriptions
- Status tracking (armed/triggered/expired/cancelled) creates a clean lifecycle
- Composite triggers can span multiple agents' domains -- these should be visible to the coordination layer
- Expiry dates and "last checked" timestamps prevent stale triggers

---

## 10. Long-Term Thesis Tracking

### The Thesis Lifecycle

Long-term theses (multi-month to multi-year) need a different tracking system than individual trades.

```
THESIS LIFECYCLE
=================
                                    kill condition
                                    met? ──> DEAD
                                   /
HYPOTHESIS ──> RESEARCH ──> THESIS ──> ACTIVE ──> MATURE ──> RESOLVED
                  |            |         |          |           |
                  v            v         v          v           v
               dropped     not yet    position   thesis      target hit
               (weak)     actionable  entered    playing out  OR
                                                              time expired
```

### Long-Term Thesis Record Format

```
THESIS RECORD
==============
ID: THESIS-2026-005
Title: "AI Infrastructure Buildout Creates Semiconductor Supercycle"
Status: ACTIVE
Created: 2026-01-15
Last Updated: 2026-03-21

THESIS STATEMENT (1-3 sentences):
  Hyperscaler capex on AI infrastructure will drive a multi-year
  semiconductor demand cycle exceeding the 2020-2021 pandemic boom.
  The market underestimates duration and magnitude because it is
  anchoring to previous cycles.

TIMEFRAME: 18-36 months (through 2027-2028)

CONVICTION: 4/5 (high)
  History:
  | Date       | Level | Reason                                  |
  |------------|-------|-----------------------------------------|
  | 2026-01-15 | 3/5   | Initial thesis, strong but unconfirmed  |
  | 2026-02-20 | 4/5   | MSFT, GOOG capex guidance far above est |
  | 2026-03-15 | 4/5   | TSMC revenue data confirms demand       |

KEY ASSUMPTIONS:
  1. Hyperscaler capex continues at $150B+ annual run rate
     - Status: CONFIRMED (Q4 2025 earnings)
     - Kill: If 2+ hyperscalers cut capex guidance >20%

  2. No fundamental breakthrough makes current GPU arch obsolete
     - Status: HOLDING (no signs)
     - Kill: If viable alternative to transformer arch emerges

  3. Energy supply constraints don't cap datacenter buildout
     - Status: WATCHING (some pressure in Northern Virginia)
     - Kill: If >3 major datacenter projects delayed by power

  4. China export controls don't destroy demand
     - Status: MODERATE RISK
     - Kill: If controls expand to ALL advanced chips (not just cutting-edge)

EVIDENCE LOG (chronological):
  | Date       | Evidence                            | Impact    |
  |------------|-------------------------------------|-----------|
  | 2026-01-20 | NVDA datacenter rev +40% YoY        | Confirms  |
  | 2026-02-05 | Intel foundry delays again          | Reduces   |
  |            |                                     | competition|
  | 2026-02-20 | MSFT capex guidance $65B for FY26   | Strongly  |
  |            |                                     | confirms  |
  | 2026-03-10 | TSMC Feb revenue +12% YoY           | Confirms  |
  | 2026-03-18 | WSJ: Northern VA power constraints  | Risk flag |

RELATED POSITIONS:
  - Long NVDA (entered 2026-01-20, +18%)
  - Long TSMC (entered 2026-02-01, +11%)
  - Long ASML (entered 2026-02-15, +7%)

RELATED THESES:
  - THESIS-2026-008: "Energy infrastructure bottleneck" (risk to this thesis)
  - THESIS-2026-002: "China tech self-sufficiency" (tangential)

REVIEW SCHEDULE:
  - Monthly: check key assumptions against new data
  - Quarterly: full thesis review with updated model
  - On catalyst: immediate review when major data point arrives

NEXT REVIEW: 2026-04-15

COUNTERPARTY VIEW (who disagrees and why):
  "The bear case is that this is a repeat of the 2000 telecom capex
  bubble -- build it and they will come, except the revenue never
  materialized. The counterargument: unlike telecom, AI models are
  already generating revenue (Copilot, ChatGPT, search). The demand
  side is proven, not speculative."
```

### ARK Invest Approach (Institutional Example)

ARK Investment Management monitors theses through:
- Weekly portfolio and research meetings reviewing every company's underlying thesis
- 5-year valuation and revenue models per company
- Key model inputs: unit volume growth, cost declines, market adoption/penetration, share count growth, future multiples
- Thesis revision frequency tied to timeframe: short-term = weekly, long-term = quarterly

### Review Triggers (When to Revisit a Thesis)

```
THESIS REVIEW TRIGGERS
=======================
SCHEDULED:
  - Monthly light touch (key assumptions still holding?)
  - Quarterly deep review (full model update)
  - Annual thesis refresh (does the entire framework still apply?)

UNSCHEDULED (event-driven):
  - Kill condition approached or met
  - Major earnings surprise (positive or negative)
  - Policy change affecting thesis
  - New competitor or technology emergence
  - Significant position P&L movement (>20% in either direction)
  - Another analyst on the team challenges the thesis
  - Related thesis changes status
```

### Why It Works

- Lifecycle model prevents theses from existing in limbo -- they must advance or die
- Evidence log creates a timestamped audit trail of what was known when
- Key assumptions with explicit kill conditions make thesis invalidation objective, not emotional
- Conviction history shows how confidence evolved (and whether it was justified)
- Counterparty view forces engagement with the bear case
- Related theses surface cross-dependencies
- Scheduled reviews prevent theses from becoming stale

### Translation to AI Agent Journal

- Each agent's `memory.md` should contain their active thesis records
- The evidence log becomes a living, append-only document
- Kill condition monitoring should be automated via the data pipeline
- Conviction history feeds the believability-weighting system
- Related theses create cross-links between agents' memories
- The lifecycle status field enables system-level dashboards showing thesis pipeline health
- Review schedule creates automatic calendar entries for agent wake-up tasks

---

## 11. Synthesis: Unified Agent Journal Schema

Drawing from all 10 frameworks above, here is a proposed unified journal schema for AI analyst agents in this project.

### Agent Memory Architecture (Three Documents, Enhanced)

```
identity.md (SLOW churn -- who the agent IS)
==============================================
- Core personality and domain expertise
- Conviction pool (paired oppositions)
- Analytical style and known biases
- Calibration summary (overall Brier score, domain scores)
- Lifetime track record summary

memory.md (MODERATE churn -- what the agent KNOWS)
====================================================
- Active thesis records (full format from Section 10)
- Watchlist (3 tiers from Section 9)
- If-then triggers (armed, from Section 6)
- Error log (from Section 4)
- Key relationships and trust assessments
- Domain knowledge graph (key causal chains)

recent.md (HIGH churn -- what just happened)
=============================================
- Last session summary
- Open trade journal entries (from Section 8)
- Pending prediction updates (from Section 7)
- Triggered alerts since last wake
- Active scenario assessments (from Section 2)
- Next-wake tasks and review calendar
```

### Core Journal Entry Types

An agent's journal consists of these structured entry types:

```
ENTRY TYPE 1: THESIS (long-lived)
  Fields from: Section 10 (Long-Term Thesis Tracking)
  + Kill conditions from: Section 6 (If-Then Planning)
  + Counterparty view from: Section 1 (Investment Memo)

ENTRY TYPE 2: PREDICTION (time-bounded)
  Fields from: Section 7 (Calibration Logs)
  + Update log with timestamps
  + Brier score on resolution

ENTRY TYPE 3: TRADE (position-level)
  Fields from: Section 8 (Trading Journal)
  + Entry/exit thesis
  + Post-trade analysis and grade

ENTRY TYPE 4: TRIGGER (conditional)
  Fields from: Section 9 (Watchlists) + Section 6 (If-Then)
  + Machine-parseable conditions
  + Lifecycle status (armed/triggered/expired)

ENTRY TYPE 5: ERROR (learning)
  Fields from: Section 4 (Dalio Error Log)
  + Root cause analysis
  + Principle extracted

ENTRY TYPE 6: OBSERVATION (daily)
  Fields from: Section 2 (Soros Diary) + Section 4 (Bridgewater Daily Obs)
  + Current macro/domain state assessment
  + Position inventory check
  + Scenario thinking (2-3 scenarios with conditional actions)

ENTRY TYPE 7: MEETING NOTE (collaborative)
  + Who participated
  + Key disagreements and their status (resolved/unresolved)
  + Decisions made and rationale
  + Action items assigned
```

### Self-Assessment System (Cross-cutting)

Drawing from Druckenmiller's "streak" tracking, Soros's "pain signal," and Dalio's believability weighting:

```
SELF-ASSESSMENT (updated weekly)
=================================
Recent accuracy:
  - Last 10 predictions resolved: X/10 directionally correct
  - Rolling Brier score (30-day): X.XX
  - Trend: [improving | stable | declining]

Domain confidence:
  - Primary domain: [confidence level]
  - Adjacent domains: [confidence levels]

Current analytical state:
  - Hot streak / cold streak / neutral
  - Highest conviction active thesis: [link]
  - Biggest current uncertainty: [description]

Anomaly flags:
  - Any prediction accuracy deterioration? [Y/N]
  - Any thesis approaching kill condition? [Y/N]
  - Any unresolved disagreement with other agents? [Y/N]
```

### Key Design Principles for the Schema

1. **Append-only for evidence and predictions** -- never overwrite, always timestamp additions
2. **Explicit kill conditions on everything** -- no thesis, trigger, or prediction lives forever without review
3. **Conviction is a number, not a word** -- enables mathematical weighting
4. **Every entry has a lifecycle status** -- active, resolved, killed, expired
5. **Cross-references between entries** -- theses link to trades, triggers link to theses, errors link to decisions
6. **Calibration data flows to identity** -- an agent's track record becomes part of who they are
7. **Machine-parseable where possible** -- triggers and predictions should be structured enough for automated monitoring
8. **Counterparty view is required** -- borrowed from the investment memo and the ACH matrix

---

## Sources

### Books & Primary Sources
- George Soros, *The Alchemy of Finance* (1987) -- real-time experiment diary
- Ray Dalio, *Principles* (2017) -- error logs, decision criteria, believability weighting
- Philip Tetlock & Dan Gardner, *Superforecasting* (2015) -- prediction tracking and calibration
- Michael Lewis, *The Big Short* (2010) -- Burry's research methodology
- Benjamin Graham & David Dodd, *Security Analysis* (1934) -- Burry's analytical foundation
- Richards Heuer, CIA *Psychology of Intelligence Analysis* -- ACH methodology
- Gary Klein, "Performing a Project Premortem," *HBR* (2007)

### Institutional & Web Sources
- [Bridgewater: 50 Years of the Daily Observations](https://www.bridgewater.com/50-years-of-the-bridgewater-daily-observations)
- [Bridgewater Tools: Dot Collector, Baseball Cards, Issue Log](https://www.principles.com/principles/a3d4f223-82d9-48ca-b12b-d00e344821c8/)
- [Point72 Academy Training Program](https://point72.com/point72-academy/)
- [CIA Tradecraft Primer: Structured Analytic Techniques](https://www.cia.gov/resources/csi/static/Tradecraft-Primer-apr09.pdf)
- [Druckenmiller on Position Sizing](https://tradebytrade.substack.com/p/druckenmillers-philosophy-on-position)
- [Druckenmiller on Liquidity and Macro](https://macro-ops.com/stanley-druckenmiller-on-liquidity-macro-margins/)
- [Soros Backache as Trading Signal](https://medium.com/activepause/investing-intuition-back-pain-8434b59872aa)
- [Burry's Silicon Investor Thread (archived)](https://greenbackd.com/2010/03/03/mike-burrys-silicon-investor-value-investing-thread/)
- [Metaculus Forecasting Platform](https://www.metaculus.com/help/prediction-resources/)
- [Good Judgment Project Track Record](https://goodjudgment.com/resources/the-superforecasters-track-record/)
- [Hedge Fund Investment Memo Format](https://daloopa.com/blog/analyst-best-practices/hedge-fund-investment-memo-example)
- [Military Decision Support Matrix](https://www.globalsecurity.org/military/library/report/call/call_00-4_ch2.htm)
- [Army Decision-Support Planning and Tools](https://www.benning.army.mil/armor/eARMOR/content/issues/2016/APR_JUN/2Klein-Hastings16.pdf)
- [Investment Thesis Tracking](https://www.sleepwellinvestments.com/p/thesis-tracker)
- [Trading Journal Best Practices](https://tradersync.com/10-data-points-to-keep-in-a-trading-journal/)
