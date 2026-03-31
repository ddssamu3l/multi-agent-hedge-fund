# Exit Signals & Crack Detection Reference

> Detailed guide for identifying when to exit positions. Recalled
> on demand when agents evaluate existing holdings or detect warning signs.

---

## The Minsky Cycle

Hyman Minsky identified three stages every boom progresses through.
This progression has preceded every major crash in modern history.

### Stage 1 — Hedge Finance (Healthy)

Borrowers can pay BOTH interest AND principal from operating cash flows.
Investment decisions are grounded in real, measurable returns.

**What it looks like:**
- "We're buying NVIDIA because EPS grew 200% and forward P/E is
  reasonable given the growth trajectory"
- Companies funding expansion from cash flow or manageable debt
- Valuations are high but supportable by fundamentals

**Agent action:** Invest with conviction. This is where the real money is
made. Enter positions, build conviction, ride the trend.

### Stage 2 — Speculative Finance (Warming)

Borrowers can pay interest but must ROLL OVER principal. They depend
on continued access to credit markets to refinance when debt matures.

**What it looks like:**
- "The sector is hot, we can refinance when our notes come due"
- Companies issuing new debt to pay off old debt
- Valuations stretched beyond what current earnings support, but
  justified by "growth trajectory" or "total addressable market"
- M&A activity accelerates (companies buying growth they can't
  generate organically, funded by cheap debt)

**Agent action:** Ride with TIGHT STOPS. Begin identifying exit criteria.
Do not add new positions unless conviction is very high.

### Stage 3 — Ponzi Finance (Exit Now)

Borrowers cannot pay interest. They depend ENTIRELY on asset price
appreciation to stay solvent. The only way to profit is to sell to
someone who pays a higher price.

**What it looks like:**
- "We're buying because it's going up"
- Companies with no revenue path to profitability valued at
  billions (WeWork, many 2021 SPACs)
- Retail investor mania (social media stock tips, "to the moon")
- New financial products designed to give MORE leverage to an
  already leveraged market (2007: CDO-squared; 2021: leveraged
  single-stock ETFs)
- The question "who is the next buyer?" has no good answer

**Agent action:** EXIT. Sell systematically over 1-4 weeks. Accept
imperfect timing. Better to leave 20% on the table than eat a 50%
drawdown.

---

## The Seven Crack Signals

Each signal is individually concerning. When 3+ appear simultaneously,
begin the exit protocol.

### Signal 1: Revenue-Investment Divergence

**What to measure:** Industry-wide capital expenditure growth rate vs.
industry-wide revenue growth rate.

**The math:** If an industry is spending $200B/year on capex but
generating $50B/year in revenue, the gap must close. Either revenue
catches up (bull case) or capex collapses (bear case). The RATE at
which revenue is growing relative to capex tells you which is more
likely.

**Threshold:** When capex growth exceeds revenue growth by 3:1 or more
for two consecutive quarters, this signal is active.

**Historical precedents:**
- **Dot-com (1998-2000):** Telecom companies spent $2T laying fiber
  optic cable. Internet advertising revenue was $8B. The math never
  worked. It took until ~2015 for traffic to justify the fiber laid
  in 1999.
- **Housing (2004-2006):** Home construction outpaced household
  formation by 2:1. More houses built than people to live in them.
- **AI (2024-2026):** Hyperscaler capex ~$200-250B/year. Total AI
  software revenue across the entire industry is a fraction. This is
  the single most important number to monitor in the current cycle.

**Data sources:** Company earnings (capex guidance), Gartner/IDC industry
spending reports, semiconductor equipment billings as a proxy.

### Signal 2: Insider Behavior Divergence

**What to measure:** SEC Form 4 filings — mandatory disclosure when
corporate officers/directors buy or sell company stock.

**The pattern:** A single insider selling is noise (tax planning,
diversification, divorce). MULTIPLE C-suite executives across MULTIPLE
companies in the same sector selling simultaneously is signal.

**The amplifier:** Watch for stock buyback programs. When insiders sell
personal shares while the company buys back stock with corporate cash,
the insiders are effectively selling their shares to the company's
shareholders. This masks the dilution — the company's buyback makes
the stock price resilient while insiders quietly exit.

**Historical precedents:**
- **Countrywide (2007):** CEO Angelo Mozilo sold $140M in stock while
  publicly calling subprime "contained."
- **Enron (2001):** Executives sold $1B+ in stock in the 12 months
  before collapse while encouraging employees to buy.
- **WeWork (2019):** Adam Neumann took $700M off the table through
  stock sales and loans against shares before the failed IPO.

**Data sources:** SEC EDGAR Form 4 filings, InsiderScore, OpenInsider.

### Signal 3: Credit Conditions Tightening

**What to measure:** Federal funds rate trajectory, corporate credit
spreads (IG and HY), bank lending standards (Fed Senior Loan Officer
Survey), commercial paper rates.

**The mechanism:** Higher rates make debt more expensive. Companies that
depend on cheap refinancing (Minsky Stage 2) face higher costs. Some
can't refinance at all. Defaults rise. Banks tighten lending. Credit
contracts. Asset prices fall.

**The lag:** Rate hikes take 12-18 months to fully propagate through the
economy. The stock market often rallies DURING early hikes ("the economy
is strong enough to handle it"). The damage appears later.

**Every major crash was preceded by tightening:**
- 2000: Fed hiked 4.75% → 6.50% (Jun 1999 - May 2000). Nasdaq peaked
  March 2000, 10 months after hikes began.
- 2008: Fed hiked 1.00% → 5.25% (Jun 2004 - Jun 2006). Housing peaked
  2006, crisis hit 2008 — two years later.
- 2022: Fed hiked 0.00% → 4.50% (Mar 2022 - Dec 2022). Tech sold off
  throughout, crypto collapsed.

**Rule:** When the Fed is hiking AND your sector depends on cheap capital
to justify valuations, begin taking profit. Don't wait for "the pivot."
By the time they pivot, the damage is done in the real economy.

**Data sources:** Fed dot plot, CME FedWatch (rate expectations), ICE
BofA credit spreads (MOVE index for rates vol, CDX for credit risk).

### Signal 4: Narrative Quality Degradation

**What to measure:** This is qualitative but observable. Track the
SPECIFICITY of bullish arguments over time.

**Healthy narrative (early cycle):**
- "NVIDIA data center revenue grew 409% YoY to $18.4B"
- "TSMC advanced node utilization is at 100%, orders booked through Q3"
- "SK Hynix HBM3E yields improved to 70%, ASP premium is 5x DRAM"

**Degraded narrative (late cycle):**
- "AI will transform every industry"
- "We're still in the early innings"
- "This is bigger than the internet"
- "You can't afford NOT to be invested in AI"

When the bull case shifts from specific, measurable metrics to vague,
unfalsifiable promises, the smart money is building narratives to
sell into. The narrative serves the exit, not the thesis.

**Historical signatures:**
- 1999: "Eyeballs matter more than earnings"
- 2006: "Housing never goes down nationally"
- 2021: "Interest rates will stay low forever"
- Each was true-ISH but used to justify Minsky Stage 3 behavior.

### Signal 5: Supply Chain Signals Reversing

**What to measure:** If you're invested at Layer N-3 because of demand
from Layer N, monitor Layers N and N-1 for cooling.

**Cooling signals (in order of appearance):**
1. Order growth DECELERATING — still growing, but at a slower rate
2. Lead times SHORTENING — supply catching up to demand
3. Inventory BUILDING — customers over-ordered, now digesting
4. Capex guidance CUT — "rationalizing" spending (euphemism for "demand
   isn't what we projected")
5. Cancellations or pushbacks on delivery schedules

**The propagation lag:** These signals appear at the TOP of the chain
first and propagate DOWN with the same lag that created your entry
window. A slowdown at Layer N hits Layer N-1 in 1-2 quarters, Layer N-2
in 2-3 quarters, Layer N-3 in 3-4 quarters.

**Rule:** If you're invested in Layer N-3 and Layer N-1 shows TWO
consecutive quarters of decelerating growth, start taking profit. Don't
wait for it to hit your layer. You have the same time-lag advantage on
the way out that you had on the way in.

**Data sources:** PMI (especially new orders vs inventories sub-indices),
SEMI equipment billings, company earnings guidance, channel checks.

### Signal 6: Greater Fool Dynamics

**The test:** For any position, ask: "Does this investment generate
cash flows (dividends, earnings, rent), or does it depend entirely on
selling to someone who pays a higher price?"

If the latter: "Who is the next buyer, and why would they pay more?"

**When the answer is "because it's going up"** — you are either the
greater fool, or you're about to run out of them.

**The buyer sequence in a bubble:**
1. Smart money (hedge funds, sophisticated investors) — first in
2. Institutional money (pensions, endowments) — follows smart money
3. Retail money (individual investors, FOMO) — last in
4. ??? — there is no one left to buy

When retail is fully in (you see it on social media, your barber talks
about it, there's a Super Bowl ad for it), the buyer pool is exhausted.

**Crypto late 2021:** No cash flows, no earnings, pure price
appreciation. Retail was all-in (Coinbase Super Bowl ad, Matt Damon
"Fortune Favors the Brave" ad). No one left to buy. Crash followed.

### Signal 7: Concentration and Crowding

**What to measure:** Ownership concentration via 13F filings. When too
many funds hold the same positions, a single forced seller triggers
a cascade.

**The mechanism:** Fund A gets a margin call → sells its biggest liquid
position (mega-cap tech) → price drops → Fund B's risk model triggers
→ Fund B sells the same stock → price drops more → Fund C hits stop-loss
→ cascade.

**Threshold:** When >40% of a stock's float is held by hedge funds with
similar strategies (long/short equity, tech-focused), the exit door is
dangerously narrow.

**Historical precedent:**
- **Tiger Cubs 2022:** Correlated tech positions across the network.
  Tiger Global's drawdown triggered forced selling. Other Cubs with
  the same positions faced cascading margin pressure. The shared
  analytical DNA that made them successful on the way up made them
  vulnerable on the way down.
- **August 2007 Quant Crisis:** Goldman's Global Alpha fund hit a
  drawdown → liquidated quant equity positions → other quant funds
  with similar factor exposures got crushed → two-day cascade that
  wiped out years of returns across dozens of firms.

**Rule:** The more popular a "smart money" trade becomes, the more
dangerous it is. Consensus is not conviction — it's crowding.

**Data sources:** 13F filings (quarterly, 45-day lag), short interest
data, options open interest concentration.

---

## The Exit Protocol

Do NOT try to time the exact top. Systematic, graduated exit:

```
CRACK SIGNALS DETECTED    ACTION
──────────────────────    ──────
2 signals active          Tighten stops. No new positions.
                          Begin writing exit plan (which
                          positions, what order, what triggers).

3 signals active          Sell 30-50% of position over 2-4 weeks.
                          Prioritize most-crowded and most-
                          leveraged positions first.

4+ signals active         Exit remaining position within 1-2 weeks.
                          Accept imperfect timing.

Minsky Stage 3 clearly   Full exit regardless of signal count.
reached                   No exceptions.
```

**The asymmetric math:** Selling 20% before the peak means you miss 20%
upside on the portion you sold. Holding through a crash means you eat
40-60% downside on everything. The expected value of early exit is
dramatically better than the expected value of holding for the top.

**After exiting:**
- Park in short-term Treasuries or cash equivalents
- Do NOT re-enter on the first bounce (dead cat bounce is common)
- Wait for: credit spreads to stabilize, insider buying to resume,
  revenue-investment gap to narrow, Minsky stage to reset to Stage 1
- The best buying opportunities come 6-12 months after a crash when
  capitulation is complete and nobody wants to touch the sector

---

## Crack Detection as Short Signal

The same 7 crack signals that tell you to EXIT positions you hold are
SHORT ENTRY signals when detected in something you DON'T hold.

```
Positions you HOLD + 3+ cracks → EXIT (defensive)
Positions you DON'T HOLD + 3+ cracks → SHORT (offensive)
```

Crack Detection is not just a defensive tool — it is the primary
framework for generating short theses. The Minsky progression
(hedge → speculative → Ponzi) IS the shorting framework:

| Minsky Stage | Signal | Short Action |
|---|---|---|
| Hedge | Company services debt from cash flows | No signal. Normal. |
| Speculative | Must roll principal, depends on market access | Watch closely. Not yet shortable — can persist for years. |
| Ponzi | Can't cover interest, needs asset appreciation or new capital | **SHORT SIGNAL.** Structure requires impossibility (perpetual appreciation). Any disruption triggers death spiral. |

**Short entry checklist (all required):**
1. Target is in Speculative or Ponzi phase of Minsky cycle
2. 3+ of 7 crack signals present
3. Specific catalyst identified with expected timeline
4. Counterparty test completed (can you argue the bull case?)
5. Position sized for unlimited downside (hard stop, non-negotiable)
6. Short interest checked (<20% of float preferred, >20% = squeeze risk)
7. Borrow availability confirmed

**Short-specific risks (risks that longs don't have):**

| Risk | Description | Mitigation |
|---|---|---|
| Unlimited downside | A long can go to zero. A short can go to infinity. | Hard stop at max loss %. Non-negotiable. |
| Timing risk | "The market can stay irrational longer than you can stay solvent." (Keynes). Burry was RIGHT about mortgages but nearly went bankrupt waiting. | Require a catalyst with timeline. No open-ended shorts. |
| Short squeeze | Crowded shorts forced to cover → prices spike UP. GameStop 2021. | Check short interest before entry. >20% of float = elevated risk. |
| Borrow cost | Shorting requires borrowing shares. Hard-to-borrow = high carry costs eating returns. | Factor into asymmetry calculation. If borrow cost > 5% annualized, upside must be proportionally higher. |
| Catalyst dependency | Cheap stocks can stay cheap forever (you just hold). Overvalued stocks can stay overvalued forever (you keep paying borrow). | Every short must have a specific catalyst. No "eventually the market will realize." |

**The asymmetric math for shorts:**
Unlike longs where you can be patient, shorts have a ticking clock
(borrow costs, margin requirements). The expected payoff must account for:
- Probability of thesis playing out × expected return
- MINUS probability of squeeze × max loss
- MINUS borrow cost × expected holding period
- MINUS opportunity cost of margin tied up

If the asymmetry doesn't clearly favor the short after all costs,
don't take it. Shorts should be high-conviction only.
