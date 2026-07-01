# Day 31 — AI Supply Chain Control Tower

**#60DayClaudeChallenge**

---

## What Was Built

An interactive, single-file HTML simulation called **AI Supply Chain Control Tower** — a real-time operations command console where the player takes on the role of Head of Operations for a global supply chain.

Alerts (port congestion, supplier delays, truck breakdowns, stockouts, customs holds, demand spikes, factory failures, weather disruptions, inventory miscounts, damaged shipments) stream into a live feed, each carrying its own countdown timer, priority level, and business impact description. Every decision — Expedite Shipment, Use Backup Supplier, Reroute Trucks, Increase Production, Transfer Inventory, Approve Air Freight, Ignore, or Delay Decision — moves eight live KPIs in different directions. Some consequences land immediately; some are delayed by design, mirroring how real operational decisions often take time to reveal their true cost.

The shift runs for 3 minutes. As the clock winds down, alert frequency increases and multiple alerts stay active simultaneously, forcing triage under pressure. At the end, the console generates a full performance report with a letter grade (A+ to D).

Built entirely offline, in one self-contained HTML file — no backend, no external libraries, no frameworks.

---

## Features & Tech Stack

| Category | Details |
|---|---|
| Core stack | HTML, CSS, Vanilla JavaScript (no React/Vue/Angular, no Tailwind/Bootstrap) |
| Architecture | Single self-contained `.html` file, fully offline after load |
| Live KPIs | Service Level, Satisfaction, Inventory Health, Transport Efficiency, Operating Cost, Revenue Protected, Score, Time Remaining |
| Alert types | 10 distinct operational disruption scenarios, each with 2–4 contextual actions |
| Decision engine | Quality-tiered actions (best / good / poor / bad) driving differentiated KPI + score deltas |
| Delayed consequences | At least one alert type resolves part of its outcome several seconds after the decision is made |
| Difficulty scaling | Spawn interval shrinks from ~5s to ~1.8s across the 3-minute shift |
| UI system | Animated glowing KPI cards, priority color coding (red/orange/cyan), pulse animation on critical alerts, scrolling event log, countdown bars |
| Extras | Sound toggle (visual-only), pause/resume modal, help/instructions modal, responsive layout for desktop and mobile |
| End-of-game report | Letter grade (A+–D), final KPI snapshot, alerts resolved, correct vs. wrong decision tally, narrative performance summary, Play Again |
| Design system | Dark navy base, glassmorphism panels, JetBrains Mono for data/timers, blue-cyan accent palette with red/orange/green priority coding |

---

## Sample Run — Final Shift Report

| Metric | Result |
|---|---|
| **Performance Grade** | **A+** |
| **Final Score** | 4,610 |
| Alerts Resolved | 51 |
| Correct Decisions | 41 |
| Wrong Decisions | 13 |
| Service Level | 100% |
| Satisfaction | 97% |
| Inventory Health | 100% |
| Transport Efficiency | 98% |
| Operating Cost | $445K |
| Revenue Protected | $124K |

> *"Exceptional shift. You anticipated risk early, protected revenue aggressively, and kept every KPI in the green. This is control-tower-grade operations leadership."*

**Toughest alert of the run:** A Demand Spike alert landed while three other critical alerts were already active on the board. With inventory and transport KPIs already under strain, the choice was between playing safe (Transfer Inventory — smaller, more certain revenue protection) or committing to scale (Increase Production — higher revenue upside, higher cost). Scaling production was the higher-risk, higher-reward call, and it paid off in the final Revenue Protected figure.

---

## Key Learnings

1. **Indecision has a price too.** Designing "Ignore" and "Delay Decision" to always carry a penalty — rather than treating them as free "safe" options — made the simulation feel truer to real operations, where unresolved risk compounds rather than disappears.

2. **Delayed consequences add depth without adding complexity.** A single alert type (inventory miscount) resolving part of its outcome a few seconds after the decision was enough to make the whole console feel more dynamic, without needing delayed logic everywhere.

3. **Difficulty curves need to be felt, not just coded.** Shrinking the spawn interval linearly across the shift created a natural mid-game lull followed by a genuinely stressful final 30 seconds — pacing that a flat spawn rate would not have produced.

4. **Grading composite scores beats single-metric scoring.** Blending KPI averages, a cost penalty, revenue protected, and raw score into one grade formula produced end-of-shift feedback that felt earned rather than arbitrary.

5. **Vanilla JS constraints sharpen design decisions.** Building without React or a framework forced simpler, more direct state management (a single `state` object, function-based rendering) — a useful contrast to the React-based simulators from earlier in the challenge.

---

## Technical Notes

- State is managed through a single centralized `state` object (KPIs, active alerts, score, timers) with all rendering handled by pure functions (`renderKpiStrip`, `renderAlertFeed`, `logEvent`) that re-paint from state rather than mutate the DOM directly.
- Alert action buttons are indexed by combining each alert's contextual actions with two universal actions (Ignore, Delay Decision) at render time, keeping the action-to-effect mapping consistent between rendering and resolution logic.
- "Delay Decision" was implemented to halve the alert's remaining time and flag it visually (⏳ + dashed highlight) rather than simply resetting its timer — this keeps delay from being a risk-free stalling tactic.
- Auto-timeout handling was unified with manual resolution through the same `resolveAlert()` function (called with an `isAuto` flag), avoiding duplicated KPI-penalty logic.
- No trademarked logos or icons were used; all iconography is generic emoji-based to stay clear of IP concerns.
- All KPI/game figures are simulation outputs only — no real supply chain data, APIs, or company data sources are used anywhere in the build.

---

## Application File

- `ai-supply-chain-control-tower.html` — single-file, offline HTML/CSS/JS build (no dependencies)

## Profile Links

- Portfolio: https://portfolio-uroojey.vercel.app/
- GitHub: https://github.com/uroojey
- LinkedIn: https://linkedin.com/in/uroojey
