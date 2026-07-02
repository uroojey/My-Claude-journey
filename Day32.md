# Day 32/60 — Think Like a Marketing Strategist: Grow This Brand

**#60DayClaudeChallenge | @Anthropic × @ABTalksOnAI × @AnilBajpai**

## What Was Built

A single-file, offline-capable React app that teaches marketing strategy by making you *do* it, not just read about it. Instead of generating captions or hashtags, the tool walks a user through the actual decision sequence a strategist follows: understand the audience → choose platforms with justified fit → pick content pillars → sequence a 30-day roadmap → react to an unplanned event → receive a Growth Report grading the decisions made along the way.

Three entry points let anyone use it, whether or not they run a business:
- 🏢 **Use My Own Business** — real business inputs
- 🙋 **Build My Personal Brand** — for people using their own name, story, and expertise as the "brand"
- 🎲 **A New Client Has Arrived** — a randomly generated business (industry, audience, budget, competitors, and a live challenge) for unlimited replay

Every stage of the app ends with a **"How to ask Claude"** card — a ready-to-copy prompt so the person learns prompt engineering alongside marketing thinking.

## Features & Tech Stack

| Feature | Details |
|---|---|
| Framework | React 18 (CDN) + Babel Standalone — single HTML file, no build step |
| Modes | Own Business / Personal Brand / Random Client generator |
| Platform logic | 8 platforms scored strong/moderate/weak fit per industry (or per personal-brand niche), each with a plain-language "why" |
| Content pillars | 8 pillars per mode, user must choose exactly 3 — each tagged with the strategic goal it supports |
| Roadmap | Auto-generated 4-week plan with weekly goals + strategy (not a post calendar) |
| Twist event | Randomized unexpected marketing event (viral post, competitor copycat, ad-cost spike, negative review, etc.) with branching consequences |
| Growth Report | Audience Understanding, Platform Strategy, Content Strategy, Growth Potential, Best Decision, Biggest Mistake, 3 Marketing Lessons |
| Design | Dark navy UI, glassmorphism cards, whiteboard-grid background, animated progress rail, sticky-note-style "Ask Claude" prompts |
| Persistence | None needed — fully replayable in-session, offline after first load |

## Sample Run — Client: North Star Labs

Ran the simulator end-to-end using the **🎲 New Client** generator.


### 3. Understanding the Audience
Client generated: **North Star Labs**, Fashion & Apparel, targeting *"trend-conscious 18–30 year olds who shop for identity, not just clothes."*


### 4. Choosing Platforms
Selected Instagram, TikTok, and Pinterest — each came back a **strong fit**, because that's where this specific audience already pays attention, not because they're the most popular platforms in general.

### 5. Choosing Content Pillars
Picked Entertainment, Promotional, and Customer Stories — a mix built to balance reach, sales, and trust instead of leaning on one lever.


### 7. The Unexpected Twist
One post unexpectedly went viral. Under pressure, I chose to **turn on ads to push it further immediately** — a plausible instinct, but not the strategist's move.

### 8. The Growth Report

**Growth Potential:** High — 3 out of 3 chosen platforms came back as strong fits.

**Audience Understanding:** Trend-conscious 18–30 year olds who shop for identity, not just clothes — a specificity meant to filter every future content and platform decision.

**Platform Strategy:** Instagram, TikTok, Pinterest — chosen for where the audience already spends attention, not platform popularity.

**Content Strategy:** Entertainment, Promotional, Customer Stories — balancing trust, reach, and connection instead of over-indexing on one.

**Response to the Unexpected:** Chose to pour ad spend into the viral moment instead of following it with real content. The report's own words: *"pressure tends to reveal the real strategy."*

**Best Decision:** Choosing Instagram, TikTok, and Pinterest — genuinely where the audience already spends attention, so effort compounds instead of fighting the algorithm.

**Biggest Mistake / Risk:** No clear misstep in the platform picks — the real risk now is inconsistency. The best strategy on paper still fails if it isn't executed week after week.


## Key Learnings

1. **Audience-first thinking is a habit, not a slide.** It's easy to define a sharp audience in a calm setup form. The real test is whether that definition still guides your decisions when something unplanned — like a viral post — happens and you're deciding in seconds, not minutes.

2. **Platform fit should be evidence-based, not popularity-based.** Building the fit-reasoning matrix (industry × platform × plain-language rationale) made it obvious how often "which platform should I use" gets answered with *what's trending* instead of *where this specific audience already is*.

3. **Strategy is revealed under pressure, not written down in advance.** The twist-event mechanic was the most valuable thing to build. A roadmap looks strategic on paper; the real strategist reveals themselves in how they react to the one thing the roadmap didn't plan for.

## Technical Notes

- Built as a single self-contained `.html` file — React 18 + Babel Standalone via CDN, no npm, no backend, no external APIs.
- Fixed a disabled-button contrast bug post-launch: `.btn-primary:disabled` originally used `opacity: 0.35` on a teal-background/near-black-text button, which faded both together into an unreadable blob against the dark UI. Replaced with explicit disabled colors (muted background + dimmed light text) to preserve contrast.
- Platform and content-pillar fit logic is fully deterministic per industry/niche, so the same inputs always produce the same reasoning — important for a teaching tool where explanations need to be trustworthy, not random.
- Random business generator can produce over 700 industry/name combinations for replay variety.


## Links

- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
