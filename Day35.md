# Day 35 — Prompt Puzzle: Master AI Prompting Through Play

**#60DayClaudeChallenge | Data Science / Analytics Domain | Intermediate Difficulty**

---

## Build Summary

Today's build flips the usual "AI helps you write prompts" script — instead, it's a **game that teaches prompt engineering by making you feel the difference** between a weak prompt and an optimized one.

Prompt Puzzle interviews the player on domain and difficulty, then generates 6 randomized scenarios drawn from real Data Science / Analytics workflows — EDA reporting, SQL optimization, churn model selection, data cleaning, BI dashboard design, and A/B test significance testing. Each scenario is wrapped in one of three challenge types, so the same underlying skill (writing a precise, well-constrained prompt) gets tested three different ways: constructing it, cleaning it, or judging it.

Everything — game logic, scenario data, scoring engine, and UI — lives in a single offline HTML file. No CDN, no backend, no dependencies.

---

## Features & Tech

| Feature | Details |
|---|---|
| **Interview-first flow** | Two-question intake (domain, difficulty) before any content generates |
| **3 Challenge Types** | Build the Prompt (drag-and-drop assembly) · Clean the Prompt (spot the distractors) · Choose the Best Prompt (weak vs. optimized vs. over-engineered) |
| **6 Randomized Scenarios** | Reshuffled every playthrough — scenario order + challenge-type mapping both randomize on replay |
| **Per-Scenario Content** | Desired Output, Correct Blocks, Distractor Blocks, Weak/Optimized/Over-Engineered Prompt, Weak/Optimized AI Output, Prompt Principle |
| **Live Scoring Engine** | Accuracy, Moves, Wrong Placements, Hints Used, Time, Optimization Bonus — tracked in real time |
| **Prompt Performance Report** | Prompt Score, Rating, Rank, Prompt DNA radar (5 dimensions), personalized feedback, next milestone, master prompt template |
| **Replay Engine** | Fully re-randomizes scenarios, block order, and challenge-type assignment; resets all stats |
| **Drag-and-Drop** | Native HTML5 drag events + click-to-move fallback for reliability across devices |
| **Micro-interactions** | Floating toast notifications, score pop animations, animated progress bar, hover lift effects |
| **Tech Stack** | Vanilla HTML/CSS/JS (no React — chosen for zero-dependency offline reliability) |
| **Design System** | Dark navy glassmorphism, JetBrains Mono for data/prompts, Inter for prose, purple-cyan accent gradient |

---

## Sample Run Results — Prompt Performance Report

```
Average Prompt Score:     110 / 100
Rank:                      💎 Diamond — AI Prompt Master
Accuracy:                  96%
Total Moves:               22
Wrong Placements:          2
Hints Used:                0
Total Time:                649s
Optimization Bonus:        +80

Prompt DNA
  Structure & Format ............. 100%
  Constraint Definition ........... 89%
  Clarity & Judgment .............. 100%
  Efficiency (speed/hints) ......... 0%
  Precision (few mistakes) ........ 80%
```

**Personalized Feedback:** Strongest skill — Structure & Format (100%). Growth edge — Efficiency (0%), a direct result of taking 649s to work through all 6 scenarios with zero hints and near-perfect accuracy. The report correctly read this as "accurate but slow," not "struggling."

**Next Milestone:** Try Advanced difficulty in a new domain to keep sharpening.

## Key Learnings

1. **Constraints are targeting instructions, not restrictions.** The single biggest "aha" across scenarios: leaving out a constraint (e.g. "prioritize interpretability") doesn't leave the AI neutral — it silently defaults to optimizing for the most common criterion (like raw accuracy), which may be exactly wrong for the audience.
2. **Distractor design is the real teaching tool.** Building believable distractor blocks — ones that sound plausible but contradict context, scope, or a stated constraint — forced far more precision than writing the "correct" blocks did.
3. **Three challenge types test three different cognitive skills.** Building a prompt tests construction; cleaning one tests discernment; choosing the best tests judgment under ambiguity. A player can be excellent at one and mediocre at another — the Prompt DNA radar exists specifically to surface that.
4. **Scoring needs to reward restraint, not just speed.** The Optimization Bonus (awarded for zero hints + zero wrong placements) meant a slower, careful player could still hit Diamond rank — accuracy without shortcuts mattered more than raw speed.
5. **Offline-first forces better engineering discipline.** No CDN, no external API calls, no fonts pulled at runtime — every interaction (drag, click, scoring, DNA visualization) had to be self-contained, which made the whole app more robust.


## Technical Notes

- Single-file architecture: HTML + `<style>` + `<script>`, no build step required — just open the file.
- Drag-and-drop implemented via native HTML5 `dragstart`/`dragover`/`drop` events, with click-to-toggle as a fallback so the game works reliably on both desktop and touch devices without extra libraries.
- Scenario objects are stored as reusable JS data structures (`RAW_SCENARIOS`), decoupled from rendering logic — new scenarios or domains can be added by extending the array, no UI changes needed.
- Full state re-render pattern: every user action mutates a central `state` object, then calls a single `render()` function — keeps the UI predictable with zero stale DOM references.
- Prompt DNA dimensions are computed heuristically per challenge type (Build → Structure & Format, Clean → Constraint Definition, Choose → Clarity & Judgment), plus derived Efficiency and Precision metrics from time/hints/wrong-placement counts.

## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
