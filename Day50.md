# Day 50 — Defend Your Experience

## Build Summary

An adaptive interview simulator that extracts every implicit claim from a resume line, project writeup, or portfolio piece, then cross-examines the user on each claim individually using the live Claude API. Unlike a fixed question bank, every follow-up question is generated from the user's actual answer, so the interview gets sharper and more specific the longer it runs. The goal is not to polish the resume; it's to pressure-test whether the person can actually defend what they've written before a real panel does it for them.

The interview adapts tone dynamically: vague or unconvincing answers get pushed harder, specific and evidenced answers get acknowledged and the interview moves on. At the end, a Defense Report scores every claim as Strong, Vague, or At Risk, with targeted advice for closing each gap.

## Features / Tech

| Feature | Details |
|---|---|
| Claim extraction | Claude API parses pasted text into 4–10 discrete, individually defensible claims, tagged as metric / strong / vague |
| Adaptive interview loop | Each follow-up question is generated from the prior answer; no fixed question bank |
| Tone calibration | Balanced mode — pressure scales up or down based on answer quality, up to 3 exchanges per claim |
| Live evaluation | Every answer scored 0–100 with a verdict (strong/weak/vague) and inline feedback |
| Confidence ring | Real-time circular progress indicator tracking % of claims well-defended |
| Claim tracker sidebar | Mini status list per claim (pending / current / strong / weak / vague) |
| Defense Report | Final breakdown: avg confidence score, claim-by-claim verdicts, and specific strengthening advice |
| Re-drill mode | One-click option to re-run the interview only on weak/vague claims |
| Session history | Stored in localStorage, viewable via a History modal |
| Export | Downloads full report as a plain-text file |
| Resilience | Retry logic with backoff for rate limits, graceful error banners, safe JSON parsing for all API responses |
| Design system | Dark navy base, glassmorphism panels, teal/cyan gradient accents, JetBrains Mono for scores and metadata |

**Stack:** Vanilla HTML/CSS/JS, single self-contained file, zero external dependencies, direct Claude API calls (`claude-sonnet-4-6`) via `fetch`.

## Sample Run Results

Tested against a data science project claim set (churn model, feature engineering, Power BI deployment):

- 7 claims extracted from a 4-sentence project description
- 2 flagged "vague" on first pass (team leadership claim, dashboard adoption claim) — both needed a specific number before the interviewer eased off
- 1 metric claim ("89% accuracy, up from 71%") was accepted after a single follow-up on validation methodology
- Average defense score across the session: 68/100 on first pass, 84/100 after re-drilling the two vague claims

## Key Learnings

- The best prep isn't rehearsing a better answer, it's discovering which of your own claims are thinner than they sound, while you still have time to fix them.
- Generic interview prep treats every claim the same. Real panels don't — they push exactly where the language gets soft ("helped with," "contributed to," "led a team") and let go the moment they hit a real number or a clear decision.
- Letting the AI's next question depend entirely on the previous answer (rather than a fixed script) is what actually makes an interview feel adaptive instead of a form with a chat UI.
- Score-then-branch logic (evaluate first, decide whether to continue or move to the next claim) needed to be a separate API call from question generation — combining them made the model's own uncertainty leak into the follow-up questions.

## Technical Notes

- Claim extraction and answer evaluation are two distinct Claude calls with different system prompts; this separation kept each call focused and improved parse reliability.
- All API responses expected in JSON are run through a safe-parse function that strips markdown fences and falls back to substring extraction between the first and last brace/bracket, since the model occasionally wraps JSON in commentary despite instructions.
- Loop termination per claim: the model returns `shouldContinue: true/false`; this is capped at a hard limit of 3 exchanges per claim as a safety fallback, not the primary control, consistent with what I've learned about agentic loop design earlier in this challenge.
- Rate limit handling uses exponential backoff (2 retries) before surfacing a dismissible, retryable error banner in the chat panel rather than failing silently.
- No CDN dependencies; the app runs fully offline aside from the Claude API calls themselves.

## Profile Links

- LinkedIn: linkedin.com/in/uroojey
- GitHub: github.com/uroojey
- Portfolio: portfolio-uroojey.vercel.app
