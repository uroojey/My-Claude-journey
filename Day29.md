# Operation Lifeline: Supply Chain Crisis Lab

A documentation overview of the simulation, the generated HTML file, the executive dashboard, and the key learnings from building it.

---

## 1. The Simulation

Operation Lifeline is a single player business simulation that puts the user in charge of a company during a supply chain crisis. Each run is procedurally generated, so the company, the crisis, and the outcome are different every time.

### Flow

The simulation runs through seven phases, tracked by a progress bar at the top of the screen.

**Phase 1: Company Briefing**
A company is generated at random: industry, annual revenue, factory count, warehouse count, supplier count, inventory days on hand, lead time, sourcing countries, founding year, employee count, and market reach. This becomes the business the user is responsible for.

**Phase 2: Crisis Alert**
One of eight crisis types fires, tied to one of the company's sourcing countries:
- Factory fire
- Supplier bankruptcy
- Port strike
- Cyberattack
- Regional flood
- Raw material shortage
- Political conflict
- Mega shipping delay

Each crisis carries a preset hit to cost, inventory, profit, delivery speed, and customer satisfaction, along with a short real world rationale for why that type of disruption matters.

**Phase 3: War Room**
The user selects exactly three response actions from six options (activate a backup supplier, emergency air freight, proactive customer communication, release safety stock, nearshore production shift, activate a crisis war room). Each action carries its own trade offs across cost, inventory, speed, and satisfaction. Choices combine with the crisis's baseline damage to produce a set of post action business metrics.

**Phase 4: Supplier Negotiation**
A four round negotiation against a single supplier: opening position, price discussion, delivery terms, and a long term agreement. Each round offers four response options that shift three running values: supplier trust, unit price, and lead time. The round four outcome rolls up into a negotiation score.

**Phase 5: CEO Boardroom**
Five executive decision questions, each with four options worth different point values (0 to 25), drawn from common crisis leadership scenarios such as investor communication, cross team misalignment, competitive poaching, capital allocation under cash pressure, and post crisis talent attrition. Each answer comes with an expert explanation. Scores combine into a leadership score.

**Phase 6: AI Strategy**
The user picks up to two AI investments from five options, each with a named capability and a stated ROI:
- Demand Forecasting AI
- Inventory Optimization
- Supplier Risk Monitor
- Warehouse Vision AI
- Procurement Copilot

**Phase 7: Mission Debrief**
The executive dashboard described in Section 3.

A replay regenerates a new company and a new crisis, so the same playthrough never repeats.

---

## 2. The Generated HTML File

The simulation is delivered as a single self contained HTML file, with no build process and no external backend.

### Stack

- **React 18**, loaded via UMD script tags rather than a bundler
- **Babel Standalone**, compiling the in browser JSX at load time
- Plain CSS, no framework, written as a custom design system in a single `<style>` block
- No state management library; all state lives in React hooks (`useState`, `useEffect`) at the component level

### Structure

- Roughly 1,500 lines in one file: `<style>` block, then a single `<script type="text/babel">` block containing data, helper functions, shared UI components, and one component per screen
- A `DATA` layer (industries, crisis templates, action definitions, negotiation rounds, boardroom questions, AI investment options) sits separately from the screen components, so the content can be edited without touching any rendering logic
- A small set of shared components are reused across screens: `PhaseBar`, `ProgressBar`, `MetricCard`, and `ScoreRing` (an animated SVG radial progress indicator)
- Screen components: `WelcomeScreen`, `CompanyScreen`, `CrisisScreen`, `WarRoomScreen`, `NegotiationScreen`, `BoardroomScreen`, `AIStrategyScreen`, `FinalDashboard`, composed inside a top level `App` component that swaps screens based on a `screen` state value

### Procedural generation

- `generateCompany()` randomly assigns an industry, revenue, factory and warehouse counts, supplier count, inventory days, lead time, and 2 to 5 sourcing countries pulled from a pool of 10
- `generateCrisis(company)` picks one of 8 crisis templates and binds it to one of that company's sourcing countries, so the crisis description is always contextual to the company that was just generated
- Combinatorially: 8 industries x 8 crisis types x 10 sourcing countries x 5 AI investment options, plus randomized company financials, means no two runs play out identically

### Design system

- Dark theme built on CSS custom properties (`--bg`, `--teal`, `--amber`, `--red`, `--green`, `--purple`, etc.) so color usage stays consistent across every component
- Typography: Inter for UI text, JetBrains Mono for numbers and scores
- Visual language: glassmorphic cards with subtle borders, colored glow states for urgency (red/amber/green/teal), animated progress bars and score rings, fade in transitions on screen change
- Fully responsive grid helpers (`grid2`, `grid3`, `grid4`) that collapse to a single column on narrow viewports

---

## 3. The Executive Dashboard

The final screen, `FinalDashboard`, is the mission debrief and the main payoff of the simulation.

### Scoring model

Six sub scores feed into one overall score:

| Sub score | Where it comes from |
|---|---|
| Leadership | Total points earned across the five boardroom questions |
| Negotiation | Score from the four round supplier negotiation |
| Resilience | Base value, boosted if the user chose backup supplier, nearshoring, or war room actions, plus a random factor |
| Cost Control | Derived from the War Room cost metric |
| Risk Management | Weighted from negotiation trust and whether a Supplier Risk Monitor was funded, plus a random factor |
| Customer Satisfaction | Derived from the War Room satisfaction metric |

The overall score is a weighted blend: leadership 25%, negotiation 20%, resilience 20%, cost control 15%, risk management 10%, customer satisfaction 10%. That overall score maps to a letter grade (A at 80+, B at 65+, C at 50+, D below that) and a title ranging from "Needs Development" to "Crisis Commander."

### Dashboard layout

- A radial score ring showing the overall score, next to the company name, the crisis name, the letter grade, and the title
- A "Performance Breakdown" grid of all six sub scores as metric cards with progress bars
- A "Personalized Feedback" section with five fixed feedback blocks:
  - Best Decision Area, the highest scoring sub score
  - Biggest Gap, the lowest scoring sub score, paired with a specific improvement tip
  - AI Strategy Impact, naming the AI investments chosen and their combined effect on profit and inventory health
  - Key Lessons Learned, four general crisis management principles
  - Expert Recommendation, a fixed 90 day action plan (risk register, dual sourcing, tabletop exercise)
- A replay button that resets state and generates a new company and crisis

The dashboard is the part of the simulation doing the actual teaching: every other phase generates a decision, the dashboard is where that decision gets tied back to a named business principle.

---

## 4. Key Learnings

### From the content itself

The simulation's built in lessons, surfaced at the end of every run, are:

1. Resilience is built before a crisis, not during one. Pre-qualifying backup suppliers and stress testing systems pays off only if it happens in advance.
2. Communication speed often matters more than communication completeness. Silence reads as incompetence even when the underlying response is sound.
3. Single source supplier dependency is a structural risk, not a minor inefficiency.
4. AI investments show their biggest return during disruption, not during steady state operations.
5. Key person dependency inside a crisis response team is itself a risk worth managing.

### From building it

- Separating the `DATA` arrays from the screen components made the simulation easy to extend. Adding a ninth crisis type or a sixth AI investment is a matter of adding one object to an array, not touching any rendering code.
- A single weighted formula for the overall score, fed by six independently computed sub scores, made the feedback section trivial to generate. Once the sub scores exist, "best area" and "biggest gap" are just a sort.
- Random number injection into otherwise deterministic scores (the `rnd()` calls in resilience and risk) keeps replays from feeling mechanical even when the same choices are made twice.
- Doing this entirely in React via UMD plus Babel Standalone, with no build tooling, kept the whole thing portable as one HTML file. The tradeoff is no code splitting and a slightly heavier initial parse, which is an acceptable cost for a project this size.
- Writing the "why this matters" context into the data layer alongside the game mechanics (crisis context, action rationale, negotiation feedback, boardroom explanations) is what turns this from a quiz into something closer to a teaching tool.

---
