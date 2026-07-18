# Day 46 — Autonomous Agent Studio

**#60DayClaudeChallenge · Build 46 of 60**

## Build Summary

Autonomous Agent Studio is a single-page HTML app that runs a live, real multi-agent orchestration pipeline against the Claude API to produce SEO-optimized long-form blog posts. Given only a topic, a fixed pre-loop (Planner → Executor) seeds a first draft, and a genuine `while` loop then calls Evaluator → Critic → Memory Manager → Improver every round — each with a live `fetch` to `https://api.anthropic.com/v1/messages` — until a runtime stop check fires: score plateau, threshold crossed, or a hard iteration cap as a safety fallback. A Final Reviewer closes the run with a publish-readiness verdict.

There is no hardcoded round count and no rule-based scoring standing in for a model call — every score, critique, and note rendered in the dashboard is the literal text of that round's API response.

## Features & Tech

| Feature | Details |
|---|---|
| Agents | Planner, Executor, Evaluator, Critic, Improver, Memory Manager, Final Reviewer (auto-designed set, no Safety Monitor — low-risk content-writing use case) |
| Orchestration | Real `while` loop; Evaluator → Critic → Memory Manager run every round; Improver runs only if the loop continues |
| Stop checks (in order) | 1. Plateau — score gain < 2 pts for 2 consecutive rounds · 2. Threshold — score ≥ user-set target · 3. Hard cap — safety fallback only |
| State threading | Running `history[]` array (round, score, breakdown, critique, memory note, draft, delta) feeds every subsequent Evaluator/Improver call |
| Live API | `fetch` to `api.anthropic.com/v1/messages`, model `claude-sonnet-4-6`, no key required in this environment |
| Error handling | Retry with backoff (up to 2 retries per call), retry counter surfaced in UI, graceful failure message on unrecoverable error |
| UI | Press-cycle SVG diagram (Improver → Evaluator return arrow, branch to Final Reviewer), live status dot, iteration history cards, activity log, dark/light theme, responsive layout |
| Stack | Vanilla HTML/CSS/JS, single self-contained file, zero external libraries/CDNs |

## Sample Run Results

I did not execute a full live browser run for this write-up — the app calls the Anthropic API directly from the browser's `fetch`, which isn't reachable from this sandboxed authoring environment. The activity log below is a **representative sample** of the log format and flow the app produces during an actual run in-browser, not a claim of real scores from a real session. Anyone opening the HTML file and clicking "Run the desk" will see live figures in this exact format.

```
[PLANNER] Building brief for topic: "How AI agents are changing content marketing"
[PLANNER] Brief complete.
[EXECUTOR] First draft written (742 words).
[EVALUATOR] Scoring round 1 draft…
[EVALUATOR] Score: 68/100
[CRITIC] Identifying highest-impact fixes…
[CRITIC] Critique ready.
[MEMORY] Distilling round into a durable note…
[MEMORY] Note stored.
[IMPROVER] Revising draft for round 2…
[IMPROVER] Revision complete.
[EVALUATOR] Scoring round 2 draft…
[EVALUATOR] Score: 81/100
[CRITIC] Identifying highest-impact fixes…
[MEMORY] Note stored.
[IMPROVER] Revising draft for round 3…
[EVALUATOR] Scoring round 3 draft…
[EVALUATOR] Score: 92/100
[STOP-CHECK] Loop stopping — reason: THRESHOLD
[FINAL REVIEWER] Verdict prepared.
```

Illustrative score trajectory: `68 → 81 → 92`, stopped by threshold (target 90) after 3 rounds, 11 live API calls total (2 setup + 3 × 3 loop calls).

## Key Learnings

1. **Stop conditions are the real design surface, not the agents.** It's tempting to spend all your effort on system prompts. The harder, higher-leverage work is deciding — precisely, in order — when the loop should stop: plateau before threshold before hard cap, so a system that's already flattened out doesn't burn rounds chasing a number it'll never hit cleanly.

2. **State has to thread forward explicitly, or the loop just repeats itself.** Each Improver call needs the prior Evaluator score *and* the Critic's specific fixes, not just the raw draft — otherwise revisions drift instead of compound.

3. **A Memory Manager earns its place by preventing re-litigation.** Its only job is a 2–3 sentence note per round: what improved, what's recurring, what not to touch again. Small footprint, but it stops the Critic from re-raising settled issues round after round.

4. **"No hardcoded rounds" is a real engineering constraint, not just a nice-to-have.** It forces every downstream agent call to be built around dynamic state (`history[]`) rather than an assumed position in a fixed sequence — which is exactly the discipline a genuinely autonomous system needs.

## Technical Notes

- **Bugs fixed:** escaped-apostrophe rendering in log/history text (handled via `escapeHtml`); fragile inline event handlers avoided in favor of `addEventListener`.
- **Testing methodology:** `node --check` on the extracted `<script>` block confirmed zero JS syntax errors before delivery. Full runtime smoke testing (live API round-trips) requires a browser session, since the app is designed to call `api.anthropic.com` directly from client-side `fetch`.
- **Retry/error handling:** each agent call retries up to 2 times with increasing backoff before surfacing a graceful failure message in the UI; retry count is tracked live in the dashboard stats.

## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
