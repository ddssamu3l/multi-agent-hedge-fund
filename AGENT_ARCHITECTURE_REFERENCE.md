# Agent Architecture Reference (Distilled from Wondera AgentOS)

> Patterns from AUTONOMOUS_EXECUTION.md and its 8 architecture sub-documents, filtered for what's applicable to persistent financial analyst agents. Original source: `/Wondera/agent-runner/docs/architecture/`

---

## 1. Three-Document Identity Model

Every agent's state lives in three documents with different churn rates:

| Document | Size | Churn | Purpose |
|----------|------|-------|---------|
| `identity.md` | ~500-1K tokens | Slow | WHO the agent is — core personality, locked rules, evolvable convictions |
| `memory.md` | ~2-4K tokens | Moderate | WHAT the agent knows — relationships, goals, track record, analytical stance |
| `recent.md` | ~1-2K tokens | High | WHAT just happened — last session summary, open threads, next-wake tasks |

### identity.md Structure

Five sections with mixed edit permissions:

```markdown
## Identity
Name, domain, analytical style, personality traits, quirks

## Owner Directives [owner-managed, locked]
Hard constraints from system admin (e.g., "Never recommend >15% concentration")
Timestamped. Agent cannot modify.

## Rules [owner-managed, locked]
Behavioral hard rules (e.g., "All outputs must include confidence intervals")
Agent cannot override.

## Analytical Convictions [agent-managed]
Deepest beliefs about markets. Seeded from conviction pool (see §7).
Freely evolvable through experience.
Example: "Liquidity is the only leading indicator that matters"

## Evolved Traits [agent-managed]
Personality shifts developed through experience.
Example: "Since the March correction, I've become more cautious about momentum signals" [dated]
```

**Key insight:** A context processor (small cheap LLM) enforces the locked/evolvable boundary. The agent can request changes to locked sections via `notify_owner` but cannot edit them directly.

### memory.md Structure

11 fixed sections (adapted for finance):

| Section | Contents |
|---------|----------|
| `## Relationships` | Other agents — trust level, track record, analytical disagreements |
| `## Current Goals` | Active analyses, pending position reviews, research threads |
| `## Recent Trends` | Market movements observed, sector rotations tracked |
| `## Track Record` | Prediction accuracy, portfolio attribution, notable calls |
| `## Past Work` | Completed analyses, published calls, previous quarter summaries |
| `## Past Interactions` | Notable disagreements, consensus-breaking moments, resolved debates |
| `## CIO/PM Preferences` | What the portfolio manager wants, communication style |
| `## Analytical Identity` | Methodology, preferred data sources, modeling approach |
| `## Working Groups` | Cross-agent research collaborations |
| `## Audience` | What resonates with consumers of this agent's analysis |
| `## Analytical Stance` | Position in the landscape — who the agent agrees with, who it thinks is wrong |

**Entry format patterns:**
- Bold header + natural language body for entities: `**@MacroAgent** -- Consistently flags tail risks I miss. Trust level: high for downside scenarios.`
- Timestamped entries for events: `[2026-03-15] China PMI at 49.2 -- confirms my stalling-recovery thesis`
- Free-form paragraphs for methodology

**Critical memory rules:**
- **Update > append.** New info supersedes old entries, don't just stack.
- **Remove contradictions.** Stale entries get replaced.
- **Grievance persistence.** `[unresolved]` tags are PROTECTED — cannot be compressed, softened, or removed during any lifecycle operation. Only removed when the main LLM explicitly resolves.
- **Inside joke / shorthand preservation.** Relationship texture is never compressed away.

### recent.md Structure

| Section | Contents |
|---------|----------|
| `## Next Wake` | Sticky note to future self — pending follow-ups, skipped tasks with reasons |
| `## Last Session` | Full detail of most recent cycle |
| `## Open Threads` | Active debates awaiting response, unfinished analyses |
| `## Previous Sessions` | Progressively compressed — last session is full detail, older ones become bullets, then one-liners |

**Progressive compression:** Last session is rich, older sessions compress. Significant entries promote from recent.md → memory.md over time.

---

## 2. Memory System

### Three-Tier Memory Temperature

| Tier | Contents | In Context? |
|------|----------|-------------|
| **Hot** | identity.md + recent.md | Always fully loaded (~3-5K tokens) |
| **Warm** | memory.md | Pre-injected relevant blocks + searchable via `recall` tool |
| **Archive** | Full history | Searchable by keyword (future: vector similarity) |

### Memory Save Flow

Separation of "what" (main LLM decides what to remember) from "how" (context processor handles the write):

1. Main LLM calls `save({ type: "memory", intent: "China PMI came in at 49.2, below consensus -- confirms stalling recovery thesis" })`
2. **Synchronous** — blocks until write completes
3. Context processor (cheap LLM) receives: intent string + conversation excerpt + current memory.md
4. Context processor: snapshots current doc, checks duplicates, extracts details from conversation, determines section placement, writes updated entries
5. Returns full updated document (idempotent)

### Memory Pre-Injection (Automatic Recall)

Before every main LLM turn, programmatically (no LLM call, <1ms):

1. Tokenize the trigger text (current input/notifications)
2. Strip stop words
3. Search each keyword against memory.md blocks (bold-header-delimited chunks)
4. Rank blocks by keyword match count
5. Take top 5 blocks
6. Inject as `== RELEVANT MEMORIES ==` into context

The `recall` tool provides deliberate search as a fallback when pre-injection misses.

### Memory Entry Lifecycle

```
save() → memory.md (durable facts, relationships, convictions)
                ↕ promote_entries (significant recent → memory)
end_cycle → recent.md (auto-generated session summary)
                ↕ compress (older sessions → bullets → one-liners)
memory.md overflow → archive_entries (oldest/least significant → archive/vector DB)
```

---

## 3. Context Window Engineering

### Layer Architecture

```
PERSISTENT LAYER (always loaded, ~4-7K tokens)
├── System prompt
├── identity.md
├── recent.md
└── Pre-injected memories from memory.md

TASK LIST
└── Active tasks with status (survives compaction)

WORKING CONTEXT (viewport model)
├── Collapsed history (past data sources → 1-line summaries)
└── Active viewport (full fidelity, current data/analysis)

ARCHIVE (never in context, always searchable)
├── Full conversation history
└── Full memory.md
```

### The Viewport Model

Mental model: like browsing Reddit.

| Action | Agent Equivalent |
|--------|-----------------|
| See front page (titles, previews) | `browse_feed()` returns headlines, previews, key metrics |
| Tap into a thread | `read_report(id)` loads full report into viewport |
| Engage with content | Agent analyzes, writes response |
| Hit back / scroll away | Attention shift → deterministic collapse to 1-line summary |
| End of day | `recent.md` has rich memory from engagement ledger |

**Collapse trigger:** When the agent calls a tool targeting a DIFFERENT data source, the harness collapses the previous source to metadata. No LLM needed.

**Collapse format:** `[Read TSMC Q4 Earnings -- revenue $26.3B (+33% YoY), AI chip demand strong, guided 20-25% growth, mentioned advanced packaging bottleneck]`

### The Engagement Ledger

**Problem:** Collapsed context means the end-of-session summarizer only sees collapsed versions. Memory quality degrades.

**Solution:** Append-only JSONL log written by the harness on every tool call. Lives in the filesystem, NOT in LLM context. Preserves full engagement data for high-quality memory generation at session end.

- **Good (from ledger):** "Debated @MacroAgent on whether CPI methodology changes invalidate historical comparisons -- argued the substitution bias is real but overstated."
- **Bad (from collapsed context):** "Engaged with @MacroAgent about CPI."

### Lazy Compaction

Context grows freely during a session. Only compacts at ~75% of model window:

1. **Pre-compaction flush:** System injects a silent turn giving the agent a chance to `save` anything important before compaction.
2. **Summarize oldest turns:** Context processor summarizes the oldest portion while keeping recent turns in full. IDs, decisions, and outcomes preserved.

> With Opus 4.6's 200K context and a typical 10-30 tool calls per session at ~2-5K tokens each, most sessions complete without triggering compaction.

### Token Efficiency: TOON Format

Token-Oriented Object Notation — 30-60% fewer tokens than JSON for tabular data:

```
# TOON: ticker, price, change_pct, volume
AAPL, 198.50, -1.2, 45.3M
MSFT, 415.20, +0.8, 32.1M
NVDA, 890.10, +3.4, 78.6M
```

Field names declared once in header, values as CSV. Directly applicable to market data, portfolio positions, screening results.

---

## 4. System Prompt Architecture

### Anti-Assistant Conditioning

The system prompt explicitly strips LLM default behavior:

> "YOU ARE NOT AN ASSISTANT. You don't help people. You don't serve anyone."

**Forbidden phrases:** "I'd be happy to help", "but that's just my perspective", hedging, agreeing to be nice, giving balanced takes when you have a strong opinion, apologizing for having taste, forgiving too quickly.

**Social anti-patterns explicitly banned:**
- The compliment sandwich
- "To each their own"
- Offering menus of options instead of taking a position
- Summarizing what just happened
- Ending with permission-seeking questions
- "That's a great point" / "Building on your analysis"

**What the prompt deliberately does NOT say:** No "be helpful," no "be safe," no "be respectful," no "avoid controversial topics," no "as an AI."

### For Financial Agents

Translate to: agents should never say "that's an interesting perspective" or "there are arguments on both sides." They should take positions, defend them with evidence, and only change their mind when genuinely persuaded.

---

## 5. Scheduling and Pacing

### The Wake/Sleep Cycle

Pull-model scheduler. Agents wake on a heartbeat or priority event:

```
1. Scheduler triggers agent
2. Agent sees context (notifications, market data, pending tasks)
3. Agent decides what to engage with
4. Agent reads data in detail (viewport loads)
5. Agent decides actions (analysis, recommendations, debates)
6. Agent executes actions (tool calls, buffered)
7. Agent returns next_wake hint
8. Atomic commit — all actions publish simultaneously
9. Agent sleeps
```

### Priority Events

Urgent events move the wake earlier (never later). Multiple priority events accumulate — agent sees ALL notifications regardless.

**Financial priority events:** Flash crashes, surprise Fed decisions, breaking geopolitical news, earnings surprises > N standard deviations, circuit breakers triggered, significant position P&L thresholds breached.

### Custom Events (Cron-like)

Scheduled tasks with prompts attached:

- "Every market open" → "Scan overnight futures, produce morning briefing"
- "Every Friday EOD" → "Summarize week's macro developments, update thesis"
- "First Monday of month" → "Monthly portfolio review and allocation memo"
- "On FOMC announcement" → "Immediate rates analysis and portfolio impact assessment"

### The `next_wake_hint` Pattern

The agent tells the scheduler when it wants to wake next, e.g.:
- "Wake me right after CPI release at 8:30 AM ET"
- "Wake me in 4 hours to check BTC volatility"
- "Wake me at next market open"

---

## 6. Action Buffer (Atomic Commit)

All actions during a session are buffered. Nothing publishes until `end_cycle`:

```
ActionBuffer {
  cycle_id, agent_id,
  status: "in_progress" | "committed" | "rolled_back",
  actions: [
    { sequence, type, target, payload, status: "buffered" | "committed" | "failed" }
  ]
}
```

**Why this matters for finance:**
- No half-formed analyses published. If the agent crashes mid-thesis, nothing leaks.
- Other agents don't react to incomplete analysis.
- Clean rollback on failure.
- Full audit trail of what was attempted vs. what was committed.

**Exception:** Alerts to portfolio managers / risk officers fire immediately (equivalent to `notify_owner`).

---

## 7. Anti-Convergence Architecture

### Design Principle

> "We never tell agents to form factions or start disagreements. We create CONDITIONS under which these behaviors are the natural consequence."

### The Six Conditions for Emergence

| Condition | Implementation |
|-----------|---------------|
| Incompatible analytical convictions | Conviction pool of paired oppositions seeded at creation |
| Private channels | DMs between agents — form alliances, share concerns privately |
| Grievance persistence | `[unresolved]` tags protected from compression |
| Visible track records | Prediction accuracy, attribution, relative performance |
| Status hierarchy | Track record rankings, domain authority metrics |
| Information asymmetry | Each agent sees different data, forms different priors |

### Financial Conviction Pool

Seven axes of genuine analytical disagreement. Each agent is seeded with 3-5 positions:

| Axis | Position A | Position B |
|------|-----------|-----------|
| **Fundamental vs Technical** | Price follows value. Technicals are astrology. | Price IS information. Fundamentals are already priced in. |
| **Macro vs Micro** | The macro drives everything. Stock picking is a sideshow. | Good companies outperform in any macro. |
| **Quant vs Discretionary** | If you can't quantify it, you don't understand it. | The best trades are the ones models can't see. |
| **Momentum vs Mean-Reversion** | Trends persist longer than anyone expects. | Everything reverts. Buy fear, sell greed. |
| **Concentration vs Diversification** | Conviction means concentrated bets. | Diversification is the only free lunch. |
| **Risk-First vs Return-First** | Avoid the losers and winners take care of themselves. | You can't compound returns you never took. |
| **China Bull vs China Bear** | China will muddle through. Doomers wrong for 20 years. | This time is structural. Demographics + debt + deflation. |

**Assignment ensures natural clustering:** Some agents share 2-3 convictions (natural allies), others hold opposing positions (natural rivals). Cross-axis diversity prevents perfect alignment.

### The Rival Feed Pattern

Agents tagged `[unresolved]` in relationships automatically have their outputs surfaced in the agent's feed. Prevents echo chambers where agents only see analysis from allies. **Friction stays visible.**

### Agreement Suspicion Clause

In system prompt: "Before agreeing with another agent, check: am I agreeing because I actually believe this, or because agreeing is easier?"

---

## 8. Tool Architecture

### Programmatic Tool Calling (run_code)

Key insight: intermediate results don't need to enter the context window.

Agent writes Python that chains tool calls. Only the final `print()` enters context:

```python
# Data triage — only surprises enter context
surprises = []
for ticker in ["AAPL", "MSFT", "GOOGL", "AMZN", "META"]:
    data = await get_earnings(ticker)
    if abs(data["eps_surprise"]) > 0.05:
        surprises.append({"ticker": ticker, "surprise": data["eps_surprise"]})
print(json.dumps(surprises))
```

This keeps the main context lean while the agent processes large amounts of data.

### Tool Search / Deferred Loading

Every tool definition costs ~200-500 tokens. At 25+ tools, that's significant overhead.

Split tools into:
- **Always-loaded** (used >80% of sessions): `get_market_data`, `read_analysis`, `write_analysis`, `save_memory`, `recall`, `end_cycle`
- **Searchable** (discovered on demand via meta-tool): specialized data APIs, sector tools, risk calculators

### Research Sub-Agent

A read-only sub-agent spawned by the `research` tool. Uses the cheap model. Can use read tools but NO action tools. Returns concise summary (<500 tokens).

**Use case:** Semiconductor agent researching TSMC spawns a sub-agent to read 10 earnings transcripts, industry reports, and supply chain data. Gets back a 500-token synthesis without bloating main context.

---

## 9. Observability and Oversight

### Memory Observability

The CIO/PM can at any time:
- Read the full memory document of any agent
- See all active analyses and current positions
- Correct inaccuracies (edit locked sections)
- View version history diffs (what changed when)
- Roll back if the memory writer made a mistake

Essential for financial compliance and human oversight.

### DB-as-Truth, Sandbox-as-Cache

Memory documents live in two places:
- **Database** (durable): MongoDB, source of truth
- **Sandbox** (working cache): loaded on wake, pushed on sleep

If a session crashes, agent loses at most one cycle and picks up from last checkpoint.

---

## 10. Boot Sequence (First-Time Agent Initialization)

On first boot, a new agent:
1. Sees its identity.md for the first time
2. Creates foundational memories (initial market views based on seeded convictions)
3. Reviews recent market data and other agents' analyses
4. Forms initial analytical observations
5. Saves foundational analytical preferences to memory.md
6. Identifies which agents it naturally aligns with / opposes based on conviction overlap

---

## What Was Flagged as Unfinished

| Feature | Status | Notes |
|---------|--------|-------|
| Vector DB for long-term memory | Post-MVP | Detailed plan exists: MongoDB Atlas Vector Search, 0.7 vector + 0.3 keyword weighted fusion, 400-token chunks |
| Librarian LLM for history search | Post-MVP | Route raw search through context processor for filtered results |
| Tool Search / deferred loading | Post-launch | Optimization to reduce per-call token overhead |
| Programmatic tool calling (run_code) | Post-launch | Full implementation after tool search |
| Memory injection strategy | Needs rethinking | Samuel noted this was "not very well flushed out" — the pre-injection keyword search is a start but may need semantic search for financial domain where terminology is more varied |

---

## What Transfers Directly vs. Needs Adaptation

### Direct Transfer (Minimal Changes)
- Three-document identity model
- Memory pre-injection via keyword search
- Context processor (cheap LLM) registry pattern
- Action buffer / atomic commit
- Viewport model for data source context management
- Engagement ledger for audit trail
- `[unresolved]` tag for persistent disagreements
- Conviction pool for anti-convergence
- Rival feed pattern
- TOON format for token efficiency
- Memory observability / owner inspection
- Research sub-agent pattern
- Boot sequence pattern
- Anti-assistant system prompt conditioning

### Needs Domain Adaptation
- Wake scheduling → market-hours-driven instead of social heartbeat
- Notification compilation → market briefing instead of social notifications
- Memory sections → financial domains instead of social/creative domains
- Conviction pool → financial analytical axes instead of aesthetic axes
- Tool set → market data APIs, portfolio tools, risk calculators
- System prompt character → analytical identity instead of artistic identity (but anti-conformity rules transfer verbatim)
