# Day 33 · Media Integrity Analyzer

## Build Summary

Today's build is a **Media Integrity Analyzer** — an interactive, single-file HTML tool that teaches media literacy through guided discovery rather than testing. Instead of quizzing users on facts they should already know, the app walks them through two hands-on challenges — spotting misleading headlines and detecting emotional manipulation — with a short concept explainer before each one, an interactive reveal, and a final personal integrity dashboard.

The design goal was to make critical thinking feel like a game, not a lecture: observe → think → reveal → reflect.

---

## Features & Tech

| Feature | Description | Tech Used |
|---|---|---|
| Concept primers | Plain-language explanation of *why* each concept matters before every challenge | Vanilla HTML/CSS |
| Headline Detective | Fictional headline + article; user judges clickworthiness and flags misleading words | Vanilla JS (DOM manipulation, dynamic word pills) |
| Emotion Detector | Fictional social post; user identifies feelings and emotionally-loaded words | Vanilla JS |
| Live Integrity Metrics | Headline Accuracy, Source Reliability, Emotional Manipulation, Audience Targeting — update in real time | CSS custom properties + animated bar fills |
| Reveal Panels | Accuracy score, highlighted mismatches, explanation, fair rewrite, key takeaway | Dynamic innerHTML rendering |
| Media Integrity Dashboard | Overall score, what was learned, biggest red flag, 3 practical habits, replay option | JS scoring logic + conic-gradient score ring |
| Theme | Violet Noir — dark editorial palette with glassmorphism cards | Pure CSS, no frameworks |
| Offline-first | Zero dependencies — no CDN, no backend, no images | Single HTML file |

**Stack:** Pure vanilla HTML, CSS, and JavaScript. No Tailwind, no npm, no APIs, no external assets — works fully offline in one file.

---

## Sample Run Results

**Challenge 1 — Headline Detective**
- Headline shown: *"Local Café Owner 'DESTROYS' City Council in Shocking Showdown Over Parking Rules"*
- Actual story: a routine public comment about a parking fee, with no confrontation
- Headline Accuracy Score: **28%** (Highly Misleading)
- Fair rewrite generated: *"Café Owner Raises Parking Fee Concerns at City Council Meeting; Review Planned"*

**Challenge 2 — Emotion Detector**
- Post shown: FOMO-driven lifestyle post using urgency and social-exclusion language
- Emotional Manipulation Score: **~75%** (High)
- Detected technique: artificial scarcity + social exclusion pressure
- Neutral rewrite generated automatically

**Final Dashboard**
- Overall Media Integrity Score calculated from all four live metrics
- Personalized "biggest red flag" generated based on the user's actual session
- Three standing media literacy habits presented as takeaways

---

## Key Learnings

1. **Misleading headlines rarely lie outright — they exaggerate stakes.** Building the scoring logic made it obvious that the gap between a headline and its article is usually about *intensity of language*, not fabricated facts. Words like "DESTROYS" or "SHOCKING" inflate a mundane event into something dramatic without technically saying anything false.

2. **Emotional manipulation follows repeatable patterns, not randomness.** Structuring the Emotion Detector scenarios around named techniques (artificial scarcity, conspiratorial urgency, shame-based comparison) showed how consistently manipulative content reuses the same handful of psychological levers — just skinned differently per audience.

3. **Teaching through discovery beats teaching through testing.** Designing the "explain → observe → guess → reveal" flow (instead of a straight quiz) made the same information feel like an insight the user found themselves, not a fact they were told. This is a UX pattern worth reusing for other literacy-style tools in this challenge.

4. **Live, visible metrics change how users engage.** Watching the four integrity metrics update in real time after each challenge made the abstract concept of "media integrity" feel measurable and concrete — turning a vague warning ("be careful what you read") into an actual score you can watch move.

5. **Randomized scenario pools keep replay meaningful.** Building three headline scenarios and three emotional post scenarios (rather than one fixed example) means the "Replay with New Scenarios" button actually delivers new content, reinforcing the lesson instead of repeating it.

---

## Profile Links

- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- Email: uroojfatima4111@gmail.com
