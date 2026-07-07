# Day 36 — Task Compass 🧭

**#60DayClaudeChallenge** | Theme: Organizational Thinking Simulation (Tech Company)


## Build Summary

**Task Compass** is a single-file HTML management simulation that teaches ownership, delegation, and collaboration — not job titles. Instead of a quiz format, the player runs through three escalating stages of realistic workplace tickets, drag-and-drop (with click-to-place fallback) assigning roles, building routing workflows, and coordinating multi-team responses to complex situations. The build closes with an **Organizational Thinking Dashboard** — a scored, personalized reflection on how the player reasons about work, rendered entirely in HTML/CSS/JS with animated bar charts and a dynamic written breakdown.

No frameworks, no backend, no external libraries. 100% offline.


## Features & Tech

| Feature | Details |
|---|---|
| Stage 1 — Who Owns This? | 3 tickets randomly drawn per playthrough from a 9-scenario pool; drag-or-tap ownership assignment; full reasoning + "may assist" roles revealed after every submission |
| Stage 2 — Task Routing | 3 fixed scenarios (3, 4, and 5-step workflows); player builds the routing order; animated card moves step-by-step through the org after submission |
| Stage 3 — Collaboration Challenge | 3 fixed scenarios; player assigns 1 primary owner + up to 4 supporting teams; reveals reasoning and a communication-flow narrative |
| Organizational Thinking Dashboard | Animated bar chart across 4 dimensions — Ownership, Delegation, Collaboration, Workflow Thinking — plus a written reflection: strongest area, over-assignment tendency, under-collaboration tendency, and one closing insight |
| Roles | 8 collectible role cards (Frontend Developer, Backend Developer, QA Engineer, Product Manager, UX Designer, Customer Support, Engineering Manager, DevOps Engineer), each with a distinct icon and accent color |
| Interaction | Native HTML5 drag-and-drop + click-to-select/click-to-place fallback for mobile reliability |
| Game feel | Glassmorphism cards, animated gradients, snap-fill drop zones, route animation, confetti completion celebration, progress pills + question dots |
| Tech stack | Vanilla HTML, CSS, JavaScript only — no React, no Tailwind, no npm, no external APIs |
| Verification | Full automated playthrough via Playwright (all 3 stages, all questions) confirmed zero console/runtime errors before delivery |


## Sample Run Results

*Illustrative example from a test playthrough — actual scores vary by player choices:*

| Dimension | Score |
|---|---|
| Ownership | 100% |
| Delegation | 67% |
| Collaboration | 58% |
| Workflow Thinking | 73% |

Sample reflection output for a run like this:
- **What you understood well:** Ownership — consistent, clear reasoning across all three tickets.
- **Over-assigning responsibility:** Mostly disciplined, with one situation where an extra team was pulled in unnecessarily.
- **Underestimating collaboration:** In at least one collaboration challenge, fewer supporting teams were assigned than the situation called for.
- **One insight:** Many real workplace problems are solved by teams rather than individuals — ownership usually means coordinating people, not doing everything alone.


## Key Learnings

1. **Ownership is a coordination question, not a "who's to blame" question.** Building the Stage 3 scenarios (a sudden sales spike, dropping satisfaction, negative reviews) made it obvious that the "primary owner" of a complex situation is rarely the person doing the technical fix — it's whoever is positioned to coordinate everyone who needs to be involved.
2. **Realistic routing order teaches sequencing intuition better than static diagrams.** Watching the animated card move step-by-step through Support → QA → Backend (or longer 5-step incident chains) made the *reasoning* behind order visible, not just the correct answer.
3. **Constraints as targeting, not restriction.** Capping supporting teams at 4 in Stage 3 forced meaningful trade-off decisions instead of "just pick everyone" — a small design constraint that made the collaboration lesson sharper.
4. **Click-to-place fallback matters as much as drag-and-drop itself.** Since the whole build runs offline in a single file, a robust click-based interaction path was essential to avoid a UI that only "feels good" on desktop.
5. **Scoring in categories, not points, changes what the game teaches.** Splitting results into Ownership / Delegation / Collaboration / Workflow Thinking (instead of one aggregate score) turned the ending into a mirror for how someone reasons, not just a pass/fail result.


## Technical Notes

- Single HTML file — no build step, no dependencies, works fully offline
- Data-driven design: all scenarios, roles, and reasoning text stored in JS objects for easy reuse/extension
- Randomized 3-of-9 selection for Stage 1 keeps replays fresh without expanding scope
- Verified via automated Playwright playthrough (mobile viewport, 420×900) — confirmed drag/drop + click fallback logic, scoring computation, and animations all execute without console or runtime errors
- Dark navy glassmorphism design system, JetBrains Mono for data/labels, Inter-style sans stack for prose — consistent with the visual identity used across this challenge

## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
