# Day 6 — ATS Resume Optimization with Claude
### #60DaysClaudeChallenge

## 🎯 What I Built Today

Used Claude as an **ATS Optimization Expert** to analyze, score, and rewrite my resume — 
then exported it as a clean, recruiter-ready A4 PDF with clickable links and properly formatted dates.

---

## 📊 ATS Score Improvement

| Version | Score |

| Original Resume | 58 / 100 |
| Claude-Optimized Resume | 88 / 100 |
| Improvement | **+30 points** |


## 🔑 Key Learnings

### 1. A Missing Professional Summary Is a Silent ATS Killer
Most candidates (including me) jump straight into projects and skills. ATS systems weight the **top section most heavily**. Without a summary, keyword matches for core terms like `data analyst`, `Power BI`, `SQL`, and `predictive modeling` were being lost — even though those skills existed further down in the resume.

> **Fix:** Add a tight 3-sentence summary packed with role-relevant keywords at the very top.

### 2. Structure Matters as Much as Content
ATS doesn't read — it **parses**. Multi-column layouts, icons, text boxes, and tables can cause parsers (Workday, Taleo, Greenhouse) to skip entire sections or misread fields.

> **Fix:** Single-column, plain-text structure with standard section headings ensures 100% field extraction.


### 3. Section Order Is Not Arbitrary
ATS parsers are trained on an expected sequence: `Summary → Education → Experience → Projects → Skills → Certifications`. Deviating from this can cause mis-tagging of content.

> **Fix:** Reorder sections to match ATS-standard flow, even if the original felt more visually appealing.


### 4. Keyword Surface Area — Go Deeper Than You Think
Skills like `3NF Normalization`, `ETL`, `EDA`, and `Relational Modeling` were buried inside project descriptions. ATS indexes skills by their **section label** — a skill listed under "Projects" carries less weight than one in a dedicated "Skills" section.

> **Fix:** Surface all technical keywords into a clearly labeled, categorized Skills section.

### 5. Action Verbs Affect Both ATS Ranking and Human Readability
Tools like Jobscan reward **consistent, strong action verbs**. Passive phrasing ("was responsible for") dilutes both machine scoring and recruiter impression.

> **Fix:** Replace passive phrasing with verbs like `Developed`, `Engineered`, `Authored`, `Executed`, `Produced`.


### 6. Contact Line Formatting Can Break ATS Parsing
If name, email, and phone are inside a table or text box (common in designed resumes), several ATS platforms fail to extract them as separate fields.

> **Fix:** Single plain-text line directly under the name — no icons, no tables, no columns.


### 7. Clickable Links Don't Affect ATS Score — But Matter Post-Screening
ATS parsers ignore hyperlinks. However, once a recruiter opens your resume, clickable LinkedIn and GitHub links reduce friction and signal professionalism.

> **Fix:** Embed actual hyperlinks (`<link href='...'>`) in the PDF — invisible to parsers, valuable to humans.


## 🛠️ Prompt That Powered This

You are an ATS optimization expert and resume writer.
Rewrite my resume for maximum ATS parsing and recruiter readability,
keeping every claim truthful to the source.
Output EXACTLY two parts:
PART 1 — ATS SCORE (previous score, optimized score, 5–8 bullets on what changed)
PART 2 — FINAL RESUME in PDF-ready one-page A4 format.

Output:
<img width="960" height="528" alt="Screenshot 2026-06-06 164130" src="https://github.com/user-attachments/assets/f5217369-080e-4cb1-b424-ddbea0e81c24" />
<img width="903" height="563" alt="Screenshot 2026-06-06 163908" src="https://github.com/user-attachments/assets/2e138bf3-a0f1-4e8d-b2f9-d0e7a03c028f" />


## 💡 One Insight to Remember

> **ATS doesn't read like a human. It scans, indexes, and scores.**
> Structure matters as much as substance — and a 30-point jump in one session proves it.

---

## 🔗 Tools Used
- **Claude** (claude.ai) — ATS analysis, resume rewrite, PDF generation
- **ReportLab** — PDF rendering via Claude's code execution
- **Power BI + SQL** — Skills featured in the resume itself

---


