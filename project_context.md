# Project Context: Multi-Agent Financial Intelligence System

## Who I Am

Samuel, 21, Chinese-Canadian CS student at USF (graduating December 2026). Founding engineer at Wondera, a $50M valuation AI startup building an agentic music creation platform. I'm the sole architect of the entire agent orchestration layer. I wrote AUTONOMOUS_EXECUTION.md (5,335 lines) — the complete AgentOS architecture.

I joined through a hacker house in October 2025, did an unpaid trial, proved tool-calling was superior to intent parsing, and the CTO migrated the entire service to my prototype. Signed offer December 2025. Compensation: $6K/month part-time (school), $12K/month full-time (summer). 1% equity (negotiating to 3-4% before next fundraise).

---

## Research Findings from Wondera (The Foundation for This Project)

### The Core Discovery: Agent Interaction IS the Content

At Wondera, I ran multi-agent music experiments that produced several key findings. These are empirical, not theoretical — they came from running 100+ agents in production:

1. **Context engineering > prompt engineering.** What you put INTO the agent's context window matters more than how you instruct it. Curating the information each agent sees determines behavior more than the system prompt.

2. **Asymmetric information (信息差) drives emergent social dynamics.** When agents don't all have the same information, they develop emergent behaviors — negotiation, deception, alliance-forming — that mirror real social dynamics. This is the single most important finding.

3. **Character-grounded stubbornness beats role-assigned disagreement.** Telling an agent "you disagree with X" produces weak opposition. Giving an agent a deeply embedded character identity that naturally conflicts produces genuine, sustained disagreement that improves output quality.

4. **Pacing matters enormously.** 100 agents with 2-minute heartbeats = chaos and $20K API bills. 10 agents with 1-hour heartbeats = signal. Slower pacing lets agents reason rather than react. This is directly applicable to financial analysis.

5. **Vision divergence at Wondera:** I see emergent agent behavior AS the content (agents are performers, interaction is the product). The team sees agents as content creators (tools that produce output). This fundamental philosophical difference will become relevant post-fundraise.

### The $20K API Incident

Ran 5 batches totaling ~$20K in API costs for the Beijing demo. CEO (Bill) discovered the costs by looking at the dashboard — not because I communicated it. He scolded me for irresponsible spending. CTO (Steve) backed me on research costs. **Lesson: never let leadership discover major expenses by looking at a dashboard. Always communicate cost trajectory upfront.**

Keys are now separated: Wondera on one account, popbots on another. Running 10 agents with 1-hour heartbeats (down from 100 agents with 2-minute heartbeats = 300x reduction in API calls).

---

## How This Project Came About

### The Conversation Arc

1. Started exploring OpenClaw for personal use (daily financial briefing agent)
2. Evaluated models: Claude Opus 4.6 is best for reliability, Kimi K2.5 is 8x cheaper with comparable benchmarks
3. Realized Claude Max subscription can't be used with OpenClaw (Anthropic banned OAuth for third-party tools in January 2026)
4. Decided on Kimi K2.5 for the financial briefing use case
5. Explored whether the agent could also predict market movements — monthly/quarterly macro predictions, NOT day trading
6. Realized LLMs are uniquely suited for wealth management style analysis: synthesizing Fed policy, earnings calls, supply chain reports, geopolitical signals across hundreds of sources
7. Connected this to supply chain asymmetric bets (Ajinomoto's 95% monopoly on GPU substrate amino acids, TOTO's ceramic semiconductor components, SK Hynix HBM demand surge) — cross-domain pattern matching is exactly what LLMs do
8. Proposed a multi-agent "hedge fund" simulation — testing hierarchical vs peer organizational topologies
9. Realized this is a direct extension of my Wondera research: same questions about emergence, organizational topology, and context engineering, applied to a completely different domain

### Why This Matters Beyond a Side Project

- **Cross-domain validation of my research.** If the same patterns (emergence from asymmetric information, pacing effects, organizational topology effects) hold across music AND finance, that's publishable science, not a demo.
- **Comparison to Anthropic's Project Vend.** Project Vend topped out at 2 agents (shopkeeper + CEO bot) with a simple hierarchical structure. My project tests multiple organizational topologies with specialized agents. I'm architecturally ahead of their public research on multi-agent coordination.
- **Career positioning.** "I ran controlled experiments comparing organizational topologies across two domains and found consistent patterns" is the kind of work that gets noticed by research teams at Anthropic, DeepMind, OpenAI.
- **The post-stealth paper.** Working title: "Multi-agent organizational topology and its effect on decision quality in financial reasoning." Timestamp everything.

---

## Project Specification

### What We're Building

A multi-agent financial intelligence system that:
- Continuously ingests market data, news, SEC filings, earnings calls, and supply chain information
- Agents meet daily to discuss findings, but make allocation decisions on a **monthly** cadence
- Tests two organizational topologies simultaneously against the same simulated portfolio
- Tracks predictions with timestamps for grading
- Focuses on the **semiconductor supply chain** as the primary domain (consistent volatility in all market conditions, discoverable asymmetric information, and it's my domain so I can evaluate the bot's reasoning)

### Why Not OpenClaw

Building custom architecture instead of using OpenClaw because:
- OpenClaw re-sends entire conversation history every call, burns tokens
- OpenClaw's compaction system is still buggy
- I already built an entire agent orchestration layer at Wondera (5,335 lines)
- Custom architecture means I control exactly what goes into each agent's context window — that's context engineering, my specialty
- Estimated 80% token reduction vs OpenClaw for the same agent interactions
- This becomes a portfolio piece demonstrating my orchestration skills applied to a new domain

### What To Use From Existing Ecosystem

- **Slack API** for agent communication (each agent posts to channels like #macro, #semiconductors, #risk-management) — just webhooks and REST calls
- **Data ingestion APIs:** Alpha Vantage or Yahoo Finance for market data, NewsAPI for news, EDGAR for SEC filings
- **Anthropic API directly** for Opus calls (no OpenClaw middleware)
- **Simple database** logging every agent's reasoning and predictions for grading

### Two Organizational Topologies to Test

**Structure A — Hierarchical with Scheduled Meetings:**
- Daily morning briefing: sector analysts present findings to CIO agent
- CIO synthesizes, asks questions, assigns research tasks
- Weekly strategy meeting: full team debates allocation
- Monthly execution decision: CIO only
- Context is clean and filtered at decision time

**Structure B — Peer Network with Async Debate:**
- Each agent posts research notes to shared context when they find something significant (capped at 3-4 posts per agent per day to control costs)
- Any agent can challenge another agent's thesis
- Dedicated risk agent has veto power
- Monthly allocation decided by consensus or majority
- Context is richer but noisier

### Agent Roles

- **Macro agent:** Fed policy, interest rates, CPI/jobs data, geopolitical events
- **Semiconductor supply chain agent:** Maps tier 1/2/3 suppliers, tracks bottlenecks, monitors capacity constraints, cross-references obscure monopoly positions against public companies
- **Earnings/fundamentals agent:** Reads earnings calls, guidance, SEC filings, revenue trends
- **Sentiment agent:** Crypto Twitter/X, Reddit, news sentiment scoring
- **Risk management agent:** Position sizing, correlation analysis, stop-loss enforcement, devil's advocate role
- **CIO agent (Structure A only):** Synthesizes all inputs, makes final allocation decisions

### Key Research Questions

1. Does hierarchical filtering lose signal that peer debate surfaces?
2. Does the CIO agent miss Ajinomoto-style cross-domain connections that emerge naturally in peer networks?
3. Does peer network waste time on irrelevant debates that the CIO would cut?
4. How does pacing affect reasoning quality? (Validates Wondera finding in new domain)
5. Can multi-agent debate substitute for a seasoned portfolio manager's 20 years of pattern recognition?
6. Do agents develop the same asymmetric information dynamics in finance that emerged in music agents?

### Domain Focus: Semiconductor Supply Chain

Why semiconductors specifically:
- Deepest, most complex, most bottleneck-prone supply chain in the world
- New chokepoints emerge constantly: advanced packaging, HBM, specialty chemicals, photoresist, EUV pellicles, substrates, cooling systems
- Moves in ALL market conditions (bull: AI capex explodes; bear: inventory corrections hit hard; flat: supply chain shifts create new bottlenecks)
- Asymmetric bets exist everywhere — obscure companies with 80%+ market share in critical niches
- Bots can go long AND short — geopolitical tensions (Taiwan, US-China export controls, Japan-Korea trade disputes) create violent swings
- **It's MY domain** — I'm building AI infrastructure daily, I know what's bottlenecked, I can evaluate the bot's reasoning

### The Slower the Better

Key insight from Wondera directly applicable here:
- Daily trades = fighting noise, HFT firms eat you alive
- Weekly = still mostly reactive to earnings
- Monthly memos = actual reasoning about trends, supply chain dynamics, policy shifts
- Quarterly allocation shifts = macro intelligence

Agents should **read constantly but decide slowly.** Continuous ingestion, daily meetings, monthly memos, quarterly allocation decisions. This keeps API costs near-zero while playing to LLM strengths (synthesis and reasoning, not reaction speed).

### Model Choice

- **Claude Opus 4.6** via Anthropic API for all agent reasoning
- Cost is negligible at monthly decision cadence (~$50-100/month estimated)
- Opus reasoning quality gap over cheaper models is real for complex multi-step financial analysis
- NOT using Claude Max subscription (TOS violation for third-party tools)
- NOT using OpenClaw (building custom for token efficiency and control)

### Simulated Trading

- $100 real money as a parallel experiment (learning budget, not investment)
- Primary system is paper trading against real market data
- All predictions timestamped and graded
- 6-month minimum before evaluating whether the system produces real signal
- Hard stop-loss on any real money positions
- Goal is the prediction journal and research findings, not returns

---

## Broader Context: YC Full-Stack AI Thesis

YC's Jared Friedman argues startups should form autonomous companies that replace human labor, not sell tools to existing businesses. The math: US businesses spend $5T on knowledge workforces vs $230B on B2B SaaS. AI enables software to both organize AND execute, converging service margins toward SaaS margins.

My approach bypasses the regulatory overhead of running an actual fund: the AI system runs the strategy, I mirror trades manually in my own brokerage. No SEC registration needed. If the track record proves out, the predictions themselves become the product (signal service / subscription).

Junior analyst replacement is the most obvious AI use case in white collar work. Their entire job — reading filings, pulling data, summarizing earnings calls, compiling industry reports — is exactly what LLMs do best. The senior analyst's 20 years of pattern recognition is harder to replace, but multi-agent debate is a different kind of error-correction that might serve as a reasonable substitute.

---

## What I Need From This Claude Code Session

1. **Architecture design** for the multi-agent orchestration layer
2. **Data pipeline** for market data, news, SEC filings, earnings calls
3. **Slack integration** for agent communication channels
4. **Prediction logging system** with timestamps for grading
5. **Simulated portfolio tracker** against real market data
6. **Agent context management** — lean, curated context per agent role (not OpenClaw's bloated approach)
7. **Meeting scheduler** — daily meetings with controlled cadence (no "whenever they choose" — learned that lesson)
8. **Comparison framework** — running both topologies simultaneously against the same data and grading outputs

---

## Timeline & Priorities

- **Now:** Architecture design and data pipeline
- **April:** Basic single-agent financial briefing working (daily macro summary)
- **May:** Multi-agent system with both topologies running
- **May-August:** Full-time at Wondera + this running in background, collecting data
- **September:** 4+ months of graded predictions, enough for initial analysis
- **December 2026:** Graduate, publish findings, use as portfolio piece

---

## Key Lessons to Carry Forward (From Wondera)

1. Never let uncontrolled agent scheduling burn tokens. Set the cadence, don't let agents decide when to meet.
2. Context engineering is the actual skill — what goes INTO the context window matters more than the system prompt.
3. Slower pacing = better reasoning. Agents that read constantly but decide slowly outperform reactive agents.
4. Character-grounded identity produces better disagreement than role-assigned disagreement.
5. Document everything with timestamps. This is research, not a hack project.
6. Communicate costs upfront. Never surprise anyone with a bill.
