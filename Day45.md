# Day 45 — AI Decision Strategist

## Build Summary

An impartial Decision Strategist built as a single-turn Claude prompt plus a generated interactive HTML report. The system interviews the user with exactly 4 sequential questions (decision + options, goal + timeline, gut instinct + blocker, biggest fear + reversibility), then synthesizes the answers into a full decision-support dashboard: option cards, a scored decision matrix with animated bars, a premortem, a 7-day test plan, and a decisive verdict.

Test case used: whether to start web development alongside data analytics/data science prep, with college placements 4-5 months away.

## Features & Tech

| Feature | Details |
|---|---|
| Interview flow | 4 sequential questions, one at a time, no analysis until all answers collected |
| Output format | Single self-contained HTML file, no external JS dependencies |
| Decision Matrix | 7 dimensions scored out of 10 per option, animated CSS bar-fill on load |
| Premortem | Top 2 options, 3 failure reasons + early warning sign + prevention action each |
| 7-Day Test Plan | Research, small experiment, one conversation, decision-day criteria |
| Shareable cards | 3 screenshot-ready mini cards: matrix summary, verdict, LinkedIn hook |
| Styling | Dark navy palette, glassmorphism-style cards, CSS variables for theming |
| Responsive | Full-width cards and stacked layout below 600px |

## Sample Run Results

**Decision tested:** Web Development vs. continuing Data Analytics/Data Science, 4-5 months before placements.

- Option A (Web Dev): 33/70
- Option B (Data Science): 57/70 — **Winner**

Verdict: Continue Data Science. Rationale: protects existing momentum and depth, and matches the gut answer already given in the interview, before external validation was sought.

## Key Learnings

1. **The interview should extract, not analyze.** Keeping all 4 questions strictly sequential, with no scoring or commentary until the end, kept the user's answers unfiltered by premature framing from the AI.

2. **A decision matrix needs justification, not just numbers.** Scores are only useful when each one is traceable to something the user actually said. Fabricated precision (a score with no grounding) breaks trust in the whole report.

3. **The real value of a decision framework is often permission, not information.** In this run, the user's gut answer showed up in question 3. The rest of the interview and the full report existed to give that answer enough evidenceto be trusted and acted on.

4. **Assumption-busting has to name the bias specifically.** Generic advice ("don't overthink it") is forgettable. Naming the exact bias at play (loss aversion, overload bias) makes the insight sit with the user afterward.

## Technical Notes

- Single HTML file, starts with `<!DOCTYPE html>`, no CDN dependencies apart from Google Fonts (Inter).
- Matrix bars animate via CSS `@keyframes` with staggered `animation-delay`, avoiding JS-driven animation entirely.
- CSS custom properties used throughout for the color system, matching the fixed palette (deep navy background, teal/blue/green/red/gold/purple accents).
- Verified structurally: all 8 required sections present, matrix bars keyed to correct option gradients, responsive breakpoint at 600px confirmed in markup.

## Profile Links

- LinkedIn: linkedin.com/in/uroojey
- GitHub: github.com/uroojey
- Portfolio: portfolio-uroojey.vercel.app
- Email: uroojfatima4111@gmail.com
