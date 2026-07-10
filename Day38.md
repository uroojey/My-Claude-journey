# Day 38:  Typing Speed Studio
### #60DayClaudeChallenge

*Catching up for the day 38.*

## Build Summary

Typing Speed Studio is a premium, single-file typing platform built to feel like a commercial product rather than a basic typing test. It supports eight distinct typing modes and six adaptive content categories (plus eight programming languages), generating realistic practice passages dynamically instead of reusing the same static paragraph across modes.

The core idea: a typing test should adapt to *what* you're practicing for — general fluency, programming muscle memory, business writing, academic vocabulary, medical terminology, legal language, or creative prose — and it should tell you *why* you typed the way you did, not just how fast.

Every session ends in a full analytics dashboard (Monkeytype-inspired) with WPM/accuracy graphs, a keyboard error heatmap, consistency scoring, personal bests, achievement badges, and a written performance summary — all calculated locally, with no account or backend required.


## Features & Tech

| Feature | Details |
|---|---|
| Typing Modes | Time (15/30/60/120s), Word Count (25/50/100/250), Quote, Programming, Custom Text, Adaptive, Focus, Zen |
| Categories | General, Business, Academic, Medical, Legal, Creative Writing — each with dedicated word/sentence/quote banks |
| Programming Languages | HTML, CSS, JavaScript, Python, Java, C++, SQL, TypeScript |
| Live Stats | WPM, Raw WPM, Accuracy, CPM, Elapsed Time, Mistakes, Streak, Completion %, Remaining Time/Words |
| Visual Feedback | Per-character correct/incorrect/current highlighting, live caret, progress bar |
| Analytics Dashboard | WPM graph, accuracy ring chart, consistency score, error heatmap (QWERTY layout), top mistyped keys, personal bests, percentile estimate, achievement badges, written summary |
| Adaptive Engine | Tracks WPM + accuracy each session and raises/lowers a 1–5 difficulty level automatically |
| Persistence | Session history, personal bests, and achievements stored in `localStorage` — no account needed |
| Customization | 4 themes (Midnight, Carbon, Aurora, Daylight), dark/light toggle, 4 font sizes |
| Extras | WebAudio sound effects, keyboard shortcuts, pause/resume, restart, responsive layout |
| Tech Stack | Vanilla HTML, CSS, JavaScript — zero external libraries, zero CDN dependencies |


## Sample Run Results

A realistic typing pace test (147 characters at ~90ms/character) produced:

| Metric | Result |
|---|---|
| WPM | 124 |
| Raw WPM | 124 |
| Accuracy | 100% |
| Consistency | 73% |
| Duration | 14.3s |
| Percentile (est.) | ~Top 1% |
| Achievements Unlocked | First Steps, 40/60/80 WPM Club, Century Typist, Perfectionist, Flow State, Personal Best |

Cross-mode verification confirmed correct passage generation for all 8 modes, all 6 categories, and all 8 programming languages, with zero console or runtime errors across a full Playwright smoke test.


## Key Learnings

**1. Realistic WPM needs a sanity ceiling, not just a formula.**
A fast automated typing pass exposed a case where near-instant input produced an inflated 1,736 WPM. The formula was mathematically correct — the guard against unrealistic timing edge-cases wasn't. Adding a 350 WPM ceiling and a 1-second minimum elapsed window before final calculation fixed this without affecting real typists.

**2. Domain content is a bigger lift than domain UI.**
Building six category-specific word banks and sentence pools (Medical ≠ Legal ≠ Creative) took far more design thought than the mode-switching logic itself. Realistic domain text needed actual sentence construction, not just word-shuffling.

**3. "Focus Mode" is a UX approximation, not a literal rendering.**
True single-line visibility would need real DOM line detection (font-dependent, resize-dependent). Approximating it with word-distance-based opacity gave a close, performant result without the fragility of measuring actual rendered line breaks.

**4. Analytics should explain, not just report.**
Separating raw WPM from net WPM (to show correction cost), scoring consistency via WPM sample variance, and mapping errors onto a keyboard turned the dashboard from a scoreboard into a diagnostic tool.


## Technical Notes

- **Bug fixed:** CSS variable typo in the Carbon theme (`--text-dim` was malformed during initial authoring) — caught and corrected before final testing.
- **Bug fixed:** WPM calculation lacked an upper bound, allowing timing edge-cases to render unrealistic values (1,736 WPM in one automated test). Resolved with a 350 WPM ceiling and a 1-second minimum elapsed-time floor in the final calculation.
- **Testing:** Verified via Playwright (headless Chromium) — mode switching, live typing, pause/resume, theme switching, full session completion, dashboard rendering, and `localStorage` persistence across page reload. Zero console/runtime errors.
- **Validation:** JavaScript extracted and checked with `node --check` prior to browser testing.
- **No external dependencies:** all fonts, sounds (WebAudio-generated), and rendering (inline SVG for graphs/heatmap) are self-contained — fully reliable offline.


## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
