# Day 36 — Cognitive Pattern Explorer

## Build Summary

A calm, game-like self-reflection web app that helps users explore their own thinking and decision-making tendencies — without ever diagnosing or labeling them clinically. Built entirely with Claude as a single-file, offline HTML/CSS/vanilla JS application.

The experience runs through three chapters — scenario-based choices, a draggable priority-ranking exercise, and a draggable decision-timeline — before generating a personalized reflection with a percentage breakdown across five thinking styles: Analytical Thinker, Emotional Intuitive, Overthinking Loop Style, Action-First Decision Maker, and Balanced Reflective Thinker.

The goal was to make self-reflection feel exploratory and low-pressure — closer to a mirror than a test.

## Features & Tech

| Feature | Details |
|---|---|
| Start screen | Calm / Stress ambience toggle, sets tone before the exercise begins |
| Chapter 1 | 5 everyday scenarios, single-choice responses mapped to thinking styles |
| Chapter 2 | Draggable priority-ranking cards (mouse drag + arrow-key/button fallback) |
| Chapter 3 | Draggable decision-timeline mapping (same reusable rank-list component) |
| Final screen | Percentage breakdown bar chart, blended top-2 insight, free-text journal, downloadable `.txt` reflection |
| Accessibility | Keyboard-reorderable lists, visible focus states, live-region announcements, `prefers-reduced-motion` support |
| Scoring | Weighted point system (rank-based + choice-based), normalized with largest-remainder rounding so percentages always sum to 100% |
| Stack | Vanilla HTML/CSS/JS only — zero dependencies, fully offline, single file |

## Sample Run Results

Running through the tool myself produced this breakdown:

| Thinking Style | Score |
|---|---|
| Analytical Thinker | 34% |
| Overthinking Loop Style | 31% |
| Balanced Reflective Thinker | 18% |
| Emotional Intuitive | 11% |
| Action-First Decision Maker | 6% |

**Reflection generated:** *"You lean toward: Analytical Thinker... Your reflections suggest a blend of Analytical Thinker and Overthinking Loop Style. Together, this suggests you may move between these two rhythms depending on the situation and how much certainty you feel you need before acting."*

## Key Learnings

- **Same behavior, different roots.** A near-tie between Analytical and Overthinking Loop surfaced something worth sitting with — "gathering more data before deciding" can be genuine diligence, or it can be stalling dressed up as rigor. The pattern alone doesn't tell you which; only the person can.
- **Reflective language changes the entire feel of a tool.** Framing outputs as "you often..." and "this suggests..." rather than "you are..." kept the experience feeling exploratory instead of clinical — a small language choice with a large trust impact.
- **Percentages need care to feel honest.** Naive rounding can make a breakdown sum to 99% or 101%, which quietly undermines trust in the whole tool. Largest-remainder rounding fixed this cleanly.
- **Reusable components pay off fast.** Building one `createRankList()` function and using it for both the priority-ranking and timeline-mapping chapters cut the drag-and-drop logic in half and kept behavior consistent across the app.

## Technical Notes

- Single HTML file — no build step, no CDN, no backend, works fully offline.
- Native HTML5 drag-and-drop (`dragstart`/`dragover`/`drop`) for desktop, with visible ▲/▼ buttons and arrow-key reordering as the accessible/touch fallback — more reliable across devices than emulated touch-drag gestures.
- Scoring model: Chapter 1 choices contribute flat points per selection; Chapters 2 and 3 use rank-weighted points (5 → 1, top to bottom); totals are normalized to percentages using largest-remainder rounding so results always sum to exactly 100%.
- Verified: JS parses cleanly, no duplicate DOM IDs, balanced tag structure, responsive down to mobile widths, `prefers-reduced-motion` respected alongside a manual motion toggle.

## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
