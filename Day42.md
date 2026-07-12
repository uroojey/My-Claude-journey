# Day 42 — Personal Financial Command Center

**#60DayClaudeChallenge** · Built with [Claude](https://claude.ai)

A single-file, offline-first financial dashboard for students managing a monthly allowance. Not just an expense tracker — a full command center for understanding, managing, and improving financial health: a graded health score, envelope-based budgeting, savings goals, cash flow charts, rule-based insights, and a what-if simulator.

![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E?logo=javascript&logoColor=black)
![No Dependencies](https://img.shields.io/badge/dependencies-none-success)
![Single File](https://img.shields.io/badge/build-single--file%20HTML-blue)
![Offline First](https://img.shields.io/badge/offline-first-informational)

---

## Live File

The full app lives in [`personal-financial-command-center.html`](./personal-financial-command-center.html) in this repo. Download it and open it directly in any browser — no server, no build step, no install.

## Features

| Module | What it does |
|---|---|
| **Report Card overview** | Grades financial health A+ to F across Budgeting, Saving, Spending Control, and Consistency |
| **Allowance & income log** | Tracks every income entry with source and date |
| **Budget envelopes** | 8 editable spending categories with live fill bars |
| **Expense log** | Logs and tags expenses by envelope |
| **Savings goals** | Progress-ring tracked goals with one-tap contributions |
| **Cash flow chart** | 6-month income vs. spend vs. saved, drawn on HTML5 Canvas |
| **Insights panel** | Rule-based recommendations generated from live data |
| **What-if simulator** | Sliders projecting 6-month savings under different scenarios |
| **Tips, checklist, resources, prompts** | Financial literacy content plus copy-ready Claude prompts |

## Tech Stack

- **HTML / CSS / JavaScript** — no frameworks, no external libraries, no CDN calls
- **HTML5 Canvas** for the cash flow chart (built from scratch, no chart library)
- **LocalStorage** for persistence across sessions
- **Clipboard API** for one-tap prompt copying
- Fully responsive, with a dark/light theme toggle and a dedicated print stylesheet

## How to Run

1. Download `personal-financial-command-center.html`
2. Open it in any modern browser (Chrome, Firefox, Edge, Safari)
3. That's it — all data is stored locally in your browser via `localStorage`

No `npm install`, no server, no internet connection required after download.

## Project Structure

```
day43/
├── personal-financial-command-center.html   # the full single-file app
└── README.md                                 # this file
```

## Testing

- Verified JS syntax with `node --check` after extracting the inline `<script>` block
- Smoke-tested with a `jsdom`-based script simulating: tab navigation, adding income, adding expenses, contributing to a goal, toggling the theme, toggling the checklist, running the what-if simulator, and resetting demo data
- Result: **17/17 checks passed, 0 console errors**
- (Playwright's Chromium download was blocked by network egress rules in this environment, so `jsdom` was used as the smoke-test fallback)

## Key Learnings

1. **A single grade communicates more than ten metrics.** Reframing raw numbers into a report-card-style letter grade (A+ to F) made the dashboard's health score immediately legible, especially for a student audience already fluent in grades.
2. **Escaped apostrophes inside JS string literals are still a top bug source.** Two instances of doubled backslash escaping broke `node --check` syntax validation, caught only by extracting and validating the script separately from the HTML.
3. **Inline `onclick` handlers with dynamic string arguments are fragile.** Passing user-facing text directly into an inline `onclick="fn(this, '...')"` call created escaping risk; refactoring to a `data-*` attribute plus an index lookup was more robust and easier to maintain.
4. **`let`/`const` at script top-level do not attach to `window`.** This only surfaced during `jsdom` smoke testing (`window.STATE` was `undefined`) since function declarations do attach to `window` but block-scoped variables do not — a good reminder of a browser quirk that is easy to forget.
5. **Deterministic, rule-based insights are underrated for offline tools.** Without an API dependency, simple threshold-based logic (overspend detection, savings rate vs. a 20% benchmark, logging consistency) still produced insights that felt personalized to the data.

## Bugs Fixed During Build

- Fixed a `SyntaxError: Unexpected identifier` caused by double-escaped apostrophes in two string literals (grade captions and a resource description)
- Refactored the "Copy prompt" button away from inline-escaped strings to a `data-prompt-idx` attribute to eliminate future escaping risk

## Part of #60DayClaudeChallenge

Day 42 of 60. A new Claude-built project every day, documented in public.

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com

Built with [Claude](https://claude.ai) · #60DayClaudeChallenge #BuildInPublic
