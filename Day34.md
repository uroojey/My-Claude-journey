# Day 34 — Marketing Detective
### #60DayClaudeChallenge

A single-file, offline-first detective game that turns marketing case studies into an investigation: read the dossier, examine the evidence, and name the one mistake that sank the campaign — before the report tells you if you got it right.

---

## 1. Build Summary

Marketing Detective is a self-contained HTML application (no backend, no APIs, no database, no images, no external assets) built to teach real marketing failure patterns through gameplay instead of a slide deck.

**The flow:**

1. **Case Assignment** — a case folder opens with a typewriter reveal of the company, industry, and campaign objective.
2. **Investigation Board** — a corkboard of pinned dossier cards: company info, target audience, channels, budget allocation (animated bars), campaign metrics, customer comments, and social performance.
3. **Interactive Investigation** — three draggable evidence cards ("Supporting Clues") that the user clicks to log and can physically rearrange on the board. A progress tracker unlocks the "Solve" button once all three are examined.
4. **Solve the Case** — four possible explanations (one correct, three plausible distractors) presented as case-file choices.
5. **Case Closed** — a stamp animation ("Case Closed" or "Case Reopened") delivers the verdict.
6. **Learning Report** — the real mistake, the full explanation, the three clues that proved it, and concrete suggested improvements.

12 fully detailed fictional cases are stored in a single JavaScript array and one is loaded at random each replay (never repeating the previous case back-to-back).

---

## 2. Features & Tech

| Feature | Details |
|---|---|
| Rendering approach | Pure HTML, CSS, vanilla JavaScript — no React/CDN, chosen for offline reliability as a standalone local file |
| Visual theme | Classic Noir — sepia paper tones, gold desk-lamp glow, cork-brown board, red wax-stamp accents |
| Data model | 12 case objects in one JS array: company, industry, objective, audience, channels, budget, metrics, comments, social performance, primary mistake, 3 distractors, 3 clues, explanation, 3 improvements |
| Interactivity | Draggable evidence cards (pointer events), click-to-log clue tracking, animated progress bar, animated metric bars, typewriter text effect, stamp-drop animation |
| Accessibility | Keyboard-operable clue cards (Enter/Space), visible focus states, `prefers-reduced-motion` respected |
| Replayability | Random case selection with no-immediate-repeat logic; in-session "Cases Closed" counter |
| Dependencies | None — no npm, no images, no fonts loaded externally, no localStorage |
| File size | Single `.html` file, fully self-contained |

---

## 3. Sample Run Results

Three solved cases from this session's testing, shown exactly as the Learning Report presents them.

### Case: LumaGlow Skincare (Beauty & Skincare)
- **Reach:** 2.4M | **CTR:** 0.4% | **Engagement:** 8.9% | **Conversions:** 312 | **Sales:** $9,800
- **Verdict:** A broken, slow purchase funnel undercut strong top-of-funnel buzz — the bio link was outdated and the mobile site loaded too slowly to convert interested viewers.
- **Explanation:** Reach and engagement were excellent — the creative and targeting worked. The campaign collapsed at the very last step: a slow, outdated purchase path meant curious viewers had nowhere easy to go to actually buy. A funnel-friction problem, not an audience or channel problem.
- **Improvements:** Enable TikTok Shop / shoppable posts · optimize mobile load speed to under 2 seconds · keep one persistent, always-current "Shop Now" link with UTM tracking.

### Case: Solstice Solar Panels (Renewable Energy)
- **Reach:** 780K | **CTR:** 2.4% | **Leads:** 1,850 | **Follow-up answer rate:** 6% | **Sales:** Negligible
- **Verdict:** The lead form promised an instant, self-serve savings estimate, but every lead actually received a cold sales call — a mismatch that tanked response rates despite huge lead volume.
- **Explanation:** Lead volume was never the problem — the ad successfully drove interest. The real issue is a broken promise: people expected one experience (an instant number) and got a completely different one (a cold call), so they disengaged before any conversation could happen.
- **Improvements:** Build a real instant-estimate tool to match the ad's promise · or rewrite the ad copy to accurately set the "consultation call" expectation · segment and prioritize leads by source since intent differs by channel.

### Case: Momentum Auto Insurance (Insurance)
- **Reach:** 1.3M | **CTR:** 1.6% | **Quote starts:** 6,800 | **Quote completions:** 410 | **Sales:** Very low
- **Verdict:** An excessively long, granular online quote form caused massive drop-off between starting and completing a quote.
- **Explanation:** Ad performance clearly worked — quote starts were strong. The drop happens entirely inside the form: 82% of users abandoned at question 14 of 22, versus a competitor benchmark of 8 questions and a 2-minute completion time. A funnel-design problem, not a targeting or creative one.
- **Improvements:** Cut the form to essential fields only · add a progress bar and estimated completion time · offer a fast rough estimate before requesting full underwriting detail.

---

## 4. Key Learnings

- **The biggest recurring failure pattern across all 12 cases wasn't creative or targeting — it was the "last mile."** Nearly every case had strong reach, CTR, or engagement. The campaigns died at checkout, at a form, at a promise that didn't match reality, or at a shelf with no product on it. High funnel-top metrics can mask a completely broken funnel-bottom.
- **Trust breaks quietly, not loudly.** Hidden fees, mismatched ad promises, and inaccurate product photos didn't tank campaigns immediately — they tanked them one step later, in returns, cancellations, and churn. The damage shows up in the data you check *after* the sale, not the data you check during the campaign.
- **Designing "curiosity before revelation" changes how information lands.** Structuring the UI so clues had to be actively examined (not just read passively) made the underlying marketing lesson stick harder than a static case-study write-up would have.
- **Constraints sharpen the build.** Committing to zero dependencies (no React/CDN, no images, no libraries) forced a hand-built drag system, typewriter effect, and animated stamp entirely in vanilla JS — and made the final file provably reliable as a standalone document, with no risk of a blank screen from a failed network request.
- **A game format is a legitimate way to teach diagnostic thinking.** Multiple-choice "name the mistake" with real distractors (not obviously wrong answers) forces genuine pattern recognition instead of guessing — closer to how an actual marketing audit works.

---

## 5. Technical Notes

- Verified with `node --check` on the extracted JavaScript — zero syntax errors.
- Full user flow (assign → board → drag/examine all 3 clues → solve → confirm → report → replay) tested headlessly with Playwright/Chromium — zero console or runtime errors across multiple randomized cases.
- No duplicate DOM IDs; all 12 case objects confirmed to contain every required field (mistake, distractors, clues, explanation, improvements).
- Metric bars, progress tracker, and stamp animation all respect `prefers-reduced-motion`.

---

## 7. Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
