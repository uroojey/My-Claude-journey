# Day 17 · 60DayClaudeChallenge
## Vehicle Fuel Intelligence Dashboard — Dataset Analysis & Learnings

> **Dataset:** `day17_e85_dataset_optimised.csv` · 52 records · 5 fuel types · 9 columns  
> **Vehicles analysed:** Kia Seltos (Diesel, 800 km/mo, Age 2y) · Toyota Rumion G (Petrol E20, 8,000 km/mo, Age 1y)  
> **Tools:** Claude (Anthropic) · Pure HTML/SVG dashboard · No external libraries  
> **Challenge Day:** 17 of 60 · #60DayClaudeChallenge

---

## Table of Contents

1. [Dashboard Screenshots](#dashboard-screenshots)
2. [Generated Files](#generated-files)
3. [Dataset Overview](#dataset-overview)
4. [Key Metrics Computed](#key-metrics-computed)
5. [Key Insights](#key-insights)
6. [The E85 Paradox — Deep Dive](#the-e85-paradox--deep-dive)
7. [Vehicle-Specific Findings](#vehicle-specific-findings)
8. [Age vs Cost Analysis](#age-vs-cost-analysis)
9. [E85 Score Breakdown](#e85-score-breakdown)
10. [How Visualisation Helped](#how-visualisation-helped)
11. [Learnings & Reflections](#learnings--reflections)

---

## Dashboard Screenshots

### Kia Seltos · Diesel · Age 2y · 800 km/mo

![Kia Seltos Fuel Intelligence Dashboard]/<img width="1873" height="887" alt="Screenshot 2026-06-17 112541" src="https://github.com/user-attachments/assets/efc6f405-f2ed-426d-a322-c4febb6e2335" />
<img width="1878" height="633" alt="Screenshot 2026-06-17 112609" src="https://github.com/user-attachments/assets/89607f50-4f75-48fe-9ef7-5914dcf2e931" />
<img width="1896" height="892" alt="Screenshot 2026-06-17 112626" src="https://github.com/user-attachments/assets/d74a9984-d115-4b5a-b377-436edbff2fed" />

## Generated Files

| File | Description | Size |
|------|-------------|------|
| `fuel_dashboard.html` | Kia Seltos interactive dark-mode dashboard | Pure HTML/SVG |
| `rumion_dashboard.html` | Toyota Rumion G interactive dark-mode dashboard | Pure HTML/SVG |
| `seltos_dashboard.png` | Screenshot of Seltos dashboard | 496 KB |
| `rumion_dashboard.png` | Screenshot of Rumion dashboard | 537 KB |

**Dashboard features built:**
- 5 KPI cards per vehicle (cost/km, E85 cost/km, E85 premium, break-even price, monthly cost)
- SVG bar chart — Cost/km per fuel type with hover tooltips
- SVG doughnut chart — CO₂/km share per fuel type
- SVG line chart — Cost/km vs vehicle age (0–12 years) with age marker
- Animated semi-circular gauge — E85 Score /10
- E85 Paradox breakdown panel
- Age bucket table (New / Mid-life / Aged / Old)
- Fuel comparison cards with pros/cons
- Glassmorphism dark navy design (`#0a0f1e`) — zero CDN dependencies

---

## Dataset Overview

| Column | Description |
|--------|-------------|
| `Fuel_Type` | CNG / Diesel / E85 (Flex-Fuel) / Electric (EV) / Petrol (E20) |
| `Vehicle_Age_yrs` | Age of vehicle in years |
| `Distance_km` | Distance driven per record |
| `Fuel_Cost_INR` | Total fuel cost in INR |
| `CO2_emitted_kg` | Total CO₂ emitted in kg |
| `Maintenance_Cost_INR` | Maintenance cost in INR |
| `Refuel_Recharge_time_min` | Time to refuel or recharge in minutes |
| `Fuel_Price_per_unit_INR` | Price per litre / unit |
| `Typical_Mileage` | Typical mileage in km/L or equivalent |

**Record distribution across fuel types:**
- CNG: 10 records (ages 2–8y)
- Diesel: 9 records (ages 3–12y)
- E85 Flex-Fuel: 11 records (ages 1–3y)
- Electric (EV): 11 records (ages 1–4y)
- Petrol (E20): 11 records (ages 3–8y)

---

## Key Metrics Computed

All metrics derived per-row, then averaged by Fuel_Type:

| Fuel Type | Avg Cost/km (₹) | Avg CO₂/km (kg) | Avg Maint/km (₹) | Avg Refuel (min) | Avg Mileage (km/L) |
|-----------|-----------------|-----------------|------------------|------------------|-------------------|
| **CNG** | 3.32 | 0.125 | 0.662 | 8 | 24.1 |
| **Diesel** | 4.67 | 0.179 | 1.005 | 5 | 19.6 |
| **E85 Flex-Fuel** | 6.37 | 0.070 | 0.460 | 5 | 12.9 |
| **Electric (EV)** | 1.75 | 0.091 | 0.233 | 45 | 6.9* |
| **Petrol (E20)** | 6.15 | 0.171 | 0.466 | 5 | 16.3 |

*EV mileage expressed as km/unit equivalent

**Formulas applied:**
```
Avg Cost/km        = Fuel_Cost_INR ÷ Distance_km
Avg CO₂/km         = CO2_emitted_kg ÷ Distance_km
Avg Maintenance/km = Maintenance_Cost_INR ÷ Distance_km
Pump Saving (E85)  = ((Petrol_price − E85_price) / Petrol_price) × 100
Running Penalty    = ((E85_cpkm − Petrol_cpkm) / Petrol_cpkm) × 100
Break-even Price   = (E85_mileage ÷ Petrol_mileage) × Petrol_price
```

---

## Key Insights

### 1. ⚡ The E85 Paradox — Pump Price ≠ Running Cost
> E85 appears **18% cheaper** at the pump (₹82/L vs ₹100/L Petrol), but its running cost per km is **3.57% more expensive** than Petrol
due to inferior mileage (12.9 vs 16.3 km/L). The break-even price for E85 is ₹79.11/L — below the current market price of ₹82/L. **E85 does not save money at current prices.**

### 2. 📈 Scale Amplifies Penalties Dramatically
> The ₹0.22/km E85 penalty looks trivial — until you apply scale:
> - At **800 km/mo** (Seltos): ₹176/month extra · ₹2,112/year
> - At **8,000 km/mo** (Rumion G): ₹1,759/month extra · **₹21,108/year**
> Same percentage difference, 10× the financial impact. **Usage volume is the critical variable.**

### 3. 🟢 CNG is the Overlooked Champion
> At ₹3.32/km, CNG is 46% cheaper than Petrol and 29% cheaper than Diesel.
>  For a Rumion G at 8,000 km/mo, switching from Petrol to CNG saves ₹22,640/month — ₹2.72 lakh per year. CNG's only real weaknesses are boot space loss and limited highway station density.

### 4. 🔋 EV wins on cost, loses on practicality at scale
> EV at ₹1.75/km is the cheapest fuel by a wide margin — 72% cheaper than Petrol.
>  But a 45-minute recharge time makes it operationally impractical for 8,000 km/month drivers. For high-mileage commercial use, the refuel time penalty outweighs the cost savings unless charging infrastructure is purpose-built.

### 5. 🛠️ Diesel has the highest maintenance cost
> Diesel costs ₹1.005/km in maintenance — 2.15× more than E85 (₹0.460/km) and 4.3× more than EV (₹0.233/km).
>  As vehicles age, this gap widens. A Diesel vehicle at 10+ years costs ₹5.29/km running + ₹1.41/km maintenance = ₹6.70/km total loaded cost, making it the most expensive aged powertrain.

### 6. 🌱 E85 wins the CO₂ battle decisively
> E85 emits just **0.070 kg CO₂/km** — 59% less than Petrol (0.171), 61% less than Diesel (0.179).
>  If carbon footprint is the primary metric, E85 is the clear winner among combustion fuels. EV comes second at 0.091 kg/km (grid-dependent).

### 7. 📅 New vehicles are always in the cheapest cost bracket
> Every fuel type shows cost creep with age. Petrol jumps from ₹5.85/km (New 0-2y) to ₹6.33/km (Aged 6-9y) — an 8.2% increase.
>  Diesel climbs from ₹4.35 to ₹5.29 (+21.6%) by Old (10+y). **Buying new locks in the lowest operating cost bracket.**

---

## The E85 Paradox — Deep Dive

```
Petrol price:        ₹100.00 / L
E85 price:           ₹82.00  / L
Pump saving:         18.00%  ← looks great

Petrol mileage:      16.27 km/L
E85 mileage:         12.87 km/L
Mileage gap:         -20.9%  ← kills the saving

Petrol cost/km:      ₹6.154
E85 cost/km:         ₹6.374
Running penalty:     +3.57%  ← you actually pay MORE

Break-even formula:  (E85_mileage / Petrol_mileage) × Petrol_price
Break-even price:    (12.87 / 16.27) × 100 = ₹79.11/L

Current E85 gap:     ₹82.00 − ₹79.11 = ₹2.89/L above break-even
                     ↳ E85 needs to fall ₹2.89/L before it makes financial sense
```

**Visual that made this click:** The SVG bar chart placed E85's cost/km bar visibly taller than Petrol's despite E85 being cheaper at the pump.
Without the visualisation, this relationship would have required three separate mental calculations to grasp. The chart made it instantaneous.

---

## Vehicle-Specific Findings

### Kia Seltos · Diesel · Age 2y · 800 km/mo

| Metric | Value |
|--------|-------|
| Cost per km | ₹4.67 |
| Monthly fuel cost | ₹3,739 |
| Annual fuel cost | ~₹44,870 |
| CO₂ per km | 0.179 kg |
| Maintenance per km | ₹1.005 |
| Refuel time | 5 min |
| Rank among fuels | 2nd cheapest (after EV) |
| Age bracket | New (0-2y) — cheapest bracket |
| E85 monthly penalty | ₹176/mo (negligible at 800 km) |
| Verdict | Solid value for mixed city-highway at moderate mileage |

### Toyota Rumion G · Petrol (E20) · Age 1y · 8,000 km/mo

| Metric | Value |
|--------|-------|
| Cost per km | ₹6.15 |
| Monthly fuel cost | ₹49,234 |
| Annual fuel cost | ~₹5.91 lakh |
| CO₂ per km | 0.171 kg |
| Maintenance per km | ₹0.466 |
| Refuel time | 5 min |
| Rank among fuels | 4th (only E85 is pricier) |
| Age bracket | New (0-2y) — cheapest bracket |
| E85 monthly penalty | ₹1,759/mo (₹21,108/year) |
| CNG switch savings | ₹22,640/mo · ₹2.72 L/yr |
| Verdict | High-volume use makes fuel choice critical — CNG is the rational switch |

### Side-by-Side Comparison

| Metric | Seltos (Diesel) | Rumion G (Petrol) |
|--------|-----------------|-------------------|
| Cost/km | ₹4.67 | ₹6.15 |
| Monthly km | 800 | 8,000 |
| Monthly cost | ₹3,739 | ₹49,234 |
| Annual cost | ₹44,870 | ₹5,90,808 |
| CO₂/km | 0.179 kg | 0.171 kg |
| Maint/km | ₹1.005 | ₹0.466 |
| Age | 2y | 1y |
| Best alt. fuel | Stay Diesel | Switch to CNG |
| E85 penalty/yr | ₹2,112 | ₹21,108 |

---

## Age vs Cost Analysis

### Cost/km by Age Bucket — All Fuels

| Fuel | New (0-2y) | Mid-life (3-5y) | Aged (6-9y) | Old (10+y) |
|------|-----------|-----------------|-------------|------------|
| CNG | ₹3.08 | ₹3.24 | ₹3.49 | — |
| Diesel | ₹4.35* | ₹4.35 | ₹4.69 | ₹5.29 |
| E85 | ₹6.31 | ₹6.67 | — | — |
| EV | ₹1.71 | ₹1.82 | — | — |
| Petrol | ₹5.85* | ₹5.85 | ₹6.33 | — |

*Extrapolated — dataset starts at age 3 for these fuels

**Age cost escalation rates:**
- CNG: +13.4% from New → Aged (6-9y)
- Diesel: +21.6% from New → Old (10+y)
- Petrol: +8.2% from New → Aged (6-9y)
- EV: +6.3% from New → Mid-life (3-5y) — slowest degradation

**Insight:** EV has the flattest cost curve with age — lower mileage-based maintenance means the vehicle stays cheap to run longer. Diesel shows the steepest cost escalation, especially after age 10.

---

## E85 Score Breakdown

Scoring methodology: **Lower metric value = Better rank = Higher score**

| Metric | Max Points | E85 Score | Petrol Score | Diesel Score | CNG Score | EV Score |
|--------|-----------|-----------|--------------|--------------|-----------|---------|
| Cost/km | 4 | 0.00 | 1.00 | 2.00 | 3.00 | **4.00** |
| CO₂/km | 3 | **3.00** | 0.75 | 0.00 | 1.50 | 2.25 |
| Refuel time | 2 | 1.50 | 1.50 | 1.50 | 0.50 | 0.00 |
| Maintenance/km | 1 | 0.75 | 0.50 | 0.00 | 0.25 | **1.00** |
| **TOTAL** | **10** | **5.25** | **3.75** | **3.50** | **5.25** | **7.25** |

**Rankings:**
1. 🥇 Electric (EV) — 7.25/10
2. 🥈 CNG — 5.25/10 (tied)
2. 🥈 E85 Flex-Fuel — 5.25/10 (tied)
4. Petrol (E20) — 3.75/10
5. Diesel — 3.50/10

E85 ties with CNG overall but for different reasons: CNG wins on cost, E85 wins on CO₂. Their scores cancel to the same total.

---

## How Visualisation Helped

### 1. Bar Chart → Exposed the E85 paradox instantly
Reading cost/km as numbers in a table, the difference between E85 (₹6.37) and Petrol (₹6.15) looks abstract. On the bar chart, E85's bar was visibly taller than Petrol's — the relationship became physically undeniable. **This was the single most counter-intuitive insight, made obvious in 1 second.**

### 2. Line Chart → Showed cost creep over time
The age-vs-cost line chart made it immediately clear that:
- EV and CNG lines are nearly flat (low cost degradation)
- Diesel has a visible upward slope, steeper after age 9
- Petrol climbs sharply after age 6
Without a time-series line chart, comparing age trajectories across 5 fuels simultaneously would require 5 separate mental models.

### 3. Doughnut Chart → Revealed CO₂ proportions at a glance
E85's tiny slice vs Diesel/Petrol's large slices communicated the carbon footprint gap before any number was read. The visual size difference (E85 = 11% of total CO₂ vs Diesel = 28%) communicated what a table of 0.070 vs 0.179 cannot.

### 4. Animated Gauge → Made the score emotionally tangible
Watching the E85 gauge animate to 5.25/10 — stopping precisely at mid-range — created an intuitive "middle of the pack" signal. A static number would not carry the same weight. The animation drew the eye to where it settled.

### 5. E85 Paradox Panel → Structured the counter-narrative
The sequential layout (pump price → E85 price → saving → penalty → break-even) walked through the logic step by step, making the paradox feel like a story rather than a data dump. Users process structured narrative faster than raw numbers.

### 6. KPI Cards with context tags → Prevented misreading
Each KPI card included a sub-label ("Pump cheap ≠ Run cheap", "2nd cheapest") that framed the number's meaning. Without context, ₹6.37 is meaningless. With the tag "₹0.22/km pricier", it becomes immediately actionable.

---

## Learnings & Reflections

### Technical Learnings

| # | Learning |
|---|----------|
| 1 | **Pure SVG charts are powerful** — Claude built fully interactive bar, donut, line charts and a gauge without a single JS charting library. The SVG coordinate maths (viewBox scaling, stroke-dasharray for donut arcs, path-based gauges) is complex but totally achievable inline. |
| 2 | **Glassmorphism in CSS is surprisingly clean** — `backdrop-filter: blur()`, `rgba` backgrounds, and subtle `border` with low opacity create professional dark-UI aesthetics with just a few lines. |
| 3 | **Embedding screenshots as base64 in Markdown** makes the report fully self-contained — no broken image links, no external dependencies. Ideal for sharing or archiving. |
| 4 | **Computed metrics must be per-row first, then averaged** — computing `avg(cost) / avg(distance)` gives a different (wrong) result vs `avg(cost/distance)`. Row-level computation before grouping is the correct data analysis practice. |
| 5 | **Age-extrapolation is a necessary honesty flag** — when the dataset didn't contain records for certain age brackets, the dashboard explicitly noted extrapolation with an asterisk rather than silently assuming. Data transparency matters. |

### Data Analysis Learnings

| # | Learning |
|---|----------|
| 1 | **Pump price is a misleading signal** — the E85 case proves that headline price per litre obscures the actual running cost. True TCO (total cost of ownership) must always normalise to cost/km or cost/100km. |
| 2 | **Usage volume changes everything** — a 3.57% penalty at 800 km/mo (₹176) is a rounding error. The same penalty at 8,000 km/mo (₹21,108/year) is a major financial decision. Analysis must always be anchored to the user's actual usage. |
| 3 | **Multi-metric scoring reveals trade-offs clearly** — E85 scores 0/4 on cost but 3/3 on CO₂. Without a composite score, users might cherry-pick the metric that confirms their bias. A weighted rubric forces honest trade-off visibility. |
| 4 | **Age bucket analysis predicts future costs** — knowing that Diesel escalates 21.6% by age 10+ helps owners plan maintenance reserves and switch timing. Dataset age distribution matters as much as the numbers themselves. |
| 5 | **Small datasets (52 rows) can still yield strong insights** — the E85 paradox, age degradation curves, and fuel rankings emerged from just 52 well-structured records. Data quality > data quantity. |

### AI Workflow Learnings

| # | Learning |
|---|----------|
| 1 | **Prompt structure determines output quality** — specifying exact formulas, colour codes, chart types, and layout grid in the prompt produced a first-pass dashboard that required zero post-editing. Precision in prompting = precision in output. |
| 2 | **Claude computes and visualises in one pass** — no Python notebook, no Tableau, no intermediate step. The CSV → dashboard pipeline was a single conversation context. This collapses the traditional analyst workflow significantly. |
| 3 | **Vehicle-specific templating is efficient** — the same prompt structure was reused for two vehicles with different parameters. Claude adapted all numbers, colours, labels, and insights automatically. Template + parameters > repeated full prompts. |
| 4 | **Data storytelling is a distinct skill from data analysis** — the paradox panel, verdict sentences, and age insight callouts required not just computation but narrative framing. Claude's language model core made this natural; a pure BI tool would not. |
| 5 | **Iterative refinement beats perfect first prompts** — the Seltos dashboard was built first, reviewed, and lessons applied to the Rumion G dashboard (better gauge animation, CNG savings callout, high-volume alert). Shipping v1 fast and iterating is more productive than over-engineering v0. |

---

## Summary

```
Dataset:          52 records · 5 fuels · 9 columns
Vehicles:         Kia Seltos (Diesel, 800 km/mo, 2y) + Toyota Rumion G (Petrol, 8,000 km/mo, 1y)
Most surprising:  E85 is 18% cheaper at pump but 3.57% costlier per km — break-even at ₹79.11/L
Best fuel:        EV (7.25/10 score) — cheapest, lowest maintenance, moderate CO₂
Worst fuel:       Diesel (3.50/10) — highest maintenance, worst CO₂, steepest age cost curve
Hidden winner:    CNG — best combustion value, 46% cheaper than Petrol per km
High-mileage tip: At 8,000 km/mo, switching from Petrol to CNG saves ₹2.72 lakh/year
Key learning:     Visualisation turns counter-intuitive data relationships into instant clarity
AI tool:          Claude (Anthropic) — CSV ingestion → computation → dashboard → report in one session
```

---

*Day 17 of 60 · #60DayClaudeChallenge · Built with Claude by @Anthropic*  
*Tags: @ABTalksOnAI @AnilBajpai*
