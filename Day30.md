# Day 30 — Supply Chain Builder

**#60DayClaudeChallenge | Halfway Point**

A single-file, offline, AI-powered simulation that teaches supply chain fundamentals through hands-on decision-making instead of static reading. Built with React (CDN) + Babel JSX, zero backend, zero dependencies beyond the browser.

---

## What Was Built

Supply Chain Builder is an interactive teaching tool that:

1. Generates a **random company** (industry, products, countries served, demand pattern) on every playthrough.
2. Walks the user through **five real supply chain decisions**, each with a plain-English concept explanation, a "why it matters" framing, and trade-off tags before the choice is made.
3. Explains the **trade-off consequence** of every choice immediately after it's selected — no jargon, no assumed knowledge.
4. Updates **five live business metrics** (Cost, Delivery Speed, Risk, Customer Satisfaction, Sustainability) with animated progress bars after every decision.
5. Generates a final **dashboard** with an Overall Supply Chain Score (0–100), strengths, weaknesses, biggest risk, and three practical, decision-specific improvements.
6. Lets the user **replay** with a brand-new randomized company and strategy.

### Feature Table

| Feature | Details |
|---|---|
| Framework | React 18 (CDN) + Babel Standalone (JSX in-browser) |
| Styling | Custom CSS — dark navy theme, glassmorphism cards, teal/amber accents |
| State management | `useState` only, no external state libraries |
| Decisions | Suppliers, Factory Location, Warehouse Strategy, Transportation, Inventory Strategy |
| Decision options per step | 2–4 choices, each with cost/risk/speed/sustainability deltas |
| Metrics engine | Base 50/100 per metric, decisions apply clamped deltas (0–100) |
| Scoring | Weighted average across 5 metrics (cost & risk inverted since lower is better) |
| Navigation | Forward + Back (Back correctly reverses the previous metric delta) |
| Replay | Full state reset, new random company generated |
| Offline capability | Fully functional by double-clicking the HTML file — no server, no APIs |

---

## Sample Run — Cobalt Trading Co.

**Company profile:** Random generation produced *Cobalt Trading Co.*, a company serving multiple international markets with a defined demand pattern.

**Decisions made:**

| Decision | Choice |
|---|---|
| Suppliers | Multiple Suppliers |
| Factory | Nearshore Factory (Neighboring Region) |
| Warehouse | Regional Warehouse Network |
| Transport | Sea Freight |
| Inventory | Balanced Inventory |

**Final Metrics:**

| Metric | Score |
|---|---|
| Cost | 54 |
| Delivery Speed | 45 |
| Risk | 32 |
| Customer Satisfaction | 59 |
| Sustainability | 58 |

**Overall Supply Chain Score: 55 / 100** — *Solid: workable, but with room to improve.*

**Strength flagged:** Risk Level (lowest/best-performing metric)
**Weakness flagged:** None major — solidly balanced across the board
**Biggest risk identified:** No single catastrophic exposure; the build is reasonably balanced.

---

## Optimization Dashboard

This is the breakdown of *why* the score landed at 55, and which single decision moved the needle most.

### Metric-by-Metric Read

- **Risk (32 — best metric):** Driven almost entirely by the choice of **Multiple Suppliers** over a single source. Diversifying suppliers removes the single point of failure that causes most real-world supply chain collapses (strikes, shortages, quality failures at one vendor).
- **Cost (54 — mid-range):** A trade-off between the savings from Sea Freight (cheapest transport mode) and the added cost of running a Regional Warehouse Network and multiple supplier relationships.
- **Delivery Speed (45 — weakest metric):** Sea Freight is the slowest transport mode available in the simulation. This is the clearest cause-and-effect relationship in the whole run — cheap and slow, no way around it.
- **Customer Satisfaction (59):** Lifted by the Regional Warehouse Network (faster last-mile fulfillment) but capped by the slow transport choice.
- **Sustainability (58):** Helped by Sea Freight being relatively fuel-efficient per unit shipped, and by the nearshore factory keeping total travel distance moderate.

### The One Decision With the Biggest Impact

**Choosing Multiple Suppliers over a Single Supplier.**

This single choice was responsible for the largest single-metric swing in the entire run, pulling Risk down to 32 — the standout strength of the build. In a single-supplier setup, this company would have been one disruption away from a full production stop. The cost of diversification (slightly higher procurement cost, more coordination overhead) was small compared to the resilience it bought.

**Lesson:** when a company has to choose between optimizing for *lowest cost* versus *lowest catastrophic risk*, supplier diversification is almost always worth the premium. It's not a flashy decision, but it's the structural decision that determines whether the business survives a bad month.

---

## Key Learnings

1. **Trade-offs need to be taught before they're chosen, not after.** Every decision screen shows the concept and the "why it matters" framing *before* the user picks an option — this forward-explanation pattern made the tool genuinely usable for someone with zero supply chain background, not just a quiz with a scorecard bolted on.

2. **A flat base of 50/100 per metric makes deltas meaningful and comparable.** Starting every metric at neutral and applying clamped deltas per decision meant the final score always reflected the *combination* of choices, not just one dominant variable — closer to how real operational trade-offs compound.

3. **The Back button required reversing, not just resetting.** Naively resetting metrics on "Back" would have broken the live-metric illusion. Storing each decision's exact delta and inverting it on Back was the only way to keep the simulation state consistent while letting users freely revise earlier choices.

4. **Risk and Cost needed inverted scoring logic for the final score to feel honest.** A "high Risk number" and a "high Cost number" are both bad, while a "high Speed number" is good — building the score formula meant explicitly inverting two of the five metrics, otherwise a high-risk, high-cost build could score deceptively well.

5. **Randomization needs guardrails to stay coherent.** Randomizing company name, industry, countries, and demand pattern independently risked generating nonsensical combinations. Keeping each randomized field in its own clean array of pre-validated, plausible values kept every generated company believable on the first try.

---

## Technical Notes

- Single HTML file, no build step — works by opening directly in any modern browser.
- No external assets, images, or APIs — fully offline after the CDN scripts (React, ReactDOM, Babel) load once.
- All five decision steps are data-driven from a single `STEP_META` object, making the simulation easy to extend with new steps or options without touching component logic.
- Score calculation and the dashboard's strengths/weaknesses/improvements text are all generated dynamically from the final metrics object — nothing in the results dashboard is hardcoded to a specific path through the simulation.

---

## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
