# Day 40 — Fit Ledger: JD ↔ Resume Match Analyzer

## Build Summary

**Fit Ledger** is an AI assistant that tells job seekers, in one session, whether a specific job posting is worth tailoring a resume for — and exactly what to fix, in priority order, if it is. Built as a single self-contained HTML file with a live Claude API integration, following the full assistant-builder pipeline: interview → system prompt design → interface build → documentation.

The core interaction: paste a job description, upload a resume (PDF or DOCX) or paste it as text, and get back a verdict (Strong Match / Worth Tailoring / Long Shot / Skip This One), the genuine overlaps, the missing keywords, and a ranked list of concrete fixes — not generic resume advice.

This is the challenge's first build in the career/job-search domain, and the first to combine structured JSON output, native document upload to the Claude API, and fully offline client-side DOCX parsing in one app.

---

## Features & Tech

| Feature | Details |
|---|---|
| Match verdict | 0–100 score mapped to 4 labeled bands, rendered as an animated SVG dial |
| Structured recommendations | Priority-ranked fix cards, each with issue, concrete fix, category, and impact level |
| Keyword gap detection | JD requirements absent from the resume, shown as tag chips |
| Strength detection | Genuine overlaps between JD and resume, shown as tag chips |
| Resume input | Drag-and-drop PDF/DOCX upload, or a one-click "paste text instead" fallback |
| PDF handling | Sent to Claude as a native `document` content block (base64) — no client-side extraction |
| DOCX handling | Parsed fully client-side: hand-written ZIP central-directory reader + browser-native `DecompressionStream('deflate-raw')` to inflate `word/document.xml`, then `<w:t>` runs extracted to plain text |
| Error handling | Typed edge cases (`not_a_resume`, `not_a_job_description`, `insufficient_input`, `unreadable_file`) surfaced as clear in-voice messages, never a guessed verdict |
| Output contract | Strict JSON schema only — no markdown fences, no prose — so every field maps directly to a UI element |
| Tech stack | Vanilla HTML/CSS/JS, zero external libraries, zero CDN dependencies |
| Design system | Dark navy base, glassmorphism panels, teal/cyan accent, JetBrains Mono for data, system sans for prose |

---

## Sample Run Results

**Input:** Data Analyst JD (SQL, Power BI, stakeholder reporting) vs. a resume with strong Excel/Python background but no BI tool experience listed.

- **Verdict:** Worth Tailoring — 64/100
- **Overlaps:** Python, data cleaning, statistical analysis
- **Missing keywords:** Power BI, DAX, stakeholder reporting, dashboarding
- **Top-ranked fix:** *(High impact)* — "Add a Power BI or dashboarding project, even a personal one, since the JD lists it as a core requirement and the resume has zero mentions."
- **Second fix:** *(Medium impact)* — "Quantify the 'data cleaning' bullet with a number (rows processed, error rate reduced) since the JD emphasizes measurable data quality outcomes."

**Edge case tested:** Pasting a recipe instead of a JD → correctly returned `status: "error"`, type `not_a_job_description`, with a clear one-line message instead of a fabricated score.

---

## Technical Notes

- **Bug fixed:** Initial ZIP parsing attempt read local file headers sequentially instead of via the End of Central Directory record — this broke on DOCX files with data descriptors (common from streaming writers like Google Docs exports). Fixed by locating the EOCD signature from the end of the buffer and reading the central directory for accurate offsets and sizes.
- **Bug fixed:** Paragraph breaks were lost when stripping XML tags, producing a wall of run-on text. Fixed by converting `</w:p>` closing tags into newline markers before extracting `<w:t>` run contents.
- **Reliability decision:** Added a manual "paste resume text instead" fallback next to the file upload, since DOCX structure can vary enough (embedded objects, unusual XML namespaces) that parsing isn't guaranteed to succeed on every file.
- **API constraint respected:** `max_tokens: 1000` kept as instructed by the platform's handling layer; prompt design compensates by capping recommendation count and field length rather than requesting a larger budget.
- Zero console/runtime errors on smoke test across empty-state, loading-state, success-state, and error-state renders.


## Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com
