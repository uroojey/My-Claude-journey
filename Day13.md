# Day 13 — Job Search Analysis with Claude

## #60DayClaudeChallenge

---

## What I Did

Asked Claude to act as a recruiter/job search analyst:

1. Provided my professional profile (skills, target roles, salary expectations, location preferences)
2. Used Claude's job search connector to find live openings matching my profile
3. Got a structured comparison table: company, role, location, posted date, application link, match score, fit reasoning, and CTC
4. Asked Claude to act as a recruiter reviewing 1000+ applications to review my resume
5. Generated a corrected, ATS-optimized one-page PDF resume

---

## The Surprising Insight

My biggest gap wasn't a **skill** — it was a **job category mismatch**.

Most postings titled "Data Analyst" in my city were actually data-entry/MIS clerical roles, while the genuinely analytical work at my target salary band was hiding under titles like "Business Analyst" — often at larger companies or remote positions.

**Takeaway:** my technical stack (Python, SQL, Power BI, Tableau) was never the bottleneck. The search strategy — filtering by job *title* instead of job *description* — was.

---

## Resume Cleanup

While reviewing the resume, Claude also found:

- Broken/empty `\input{}` commands in the LaTeX source
- Inconsistent `\linespread` toggling causing uneven spacing
- A typo in `\section{certiFication}`
- Outdated portfolio link

After fixes: **ATS score improved to 92/100**, delivered as a clean one-page PDF.

---

## Key Takeaways

- AI-assisted job search analysis can reveal **structural** issues (market mismatch) that resume tweaks alone can't fix
- ATS optimization and content accuracy can coexist — no fabricated content was added
- Sometimes the most valuable output isn't a new skill to learn, but a **reframe of the search itself**

---

## Tools Used

- Indeed connector (via Claude) for live job search
- ReportLab for one-page PDF resume generation
- LaTeX source review and cleanup

---

## Progress

**Day 13 of 60** — #60DayClaudeChallenge


- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app/)
