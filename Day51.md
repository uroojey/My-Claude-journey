# Day 1 — FraudLens Capstone Kickoff
**#60DayClaudeChallenge | 10-Day Capstone | Product Discovery & Sprint Planning**

---

## 🎤 Discovery Interview

The session started with an existing idea rather than a blank slate, and Claude ran a one-question-at-a-time interview to shape it into a realistic 10-day v1.0.

| # | Question | My Answer |
|---|---|---|
| 1 | Existing idea or blank slate? | Fraud detection project — classification + anomaly detection on financial transactions, with visualization of normal vs. fraudulent activity |
| 2 | Dataset already chosen? | No — needed to find one |
| 3 | What kind of final product? | A web dashboard |
| 4 | Comfort with Scikit-learn/classification models? | Beginner |
| 5 | Comfort building a web dashboard (Streamlit / Power BI)? | Some exposure |
| 6 | Focused time available per day? | 1–2 hours/day |
| 7 | What does "deployed" mean to me? | Live on the web (Streamlit) **and** Power BI report file live on web |
| 8 | Power BI Desktop + publishing account already set up? | Desktop installed, no publishing account yet |
| 9 | GitHub + git comfort? | Have GitHub, git commands shaky |
| 10 | Dataset direction — common vs. less common? | Wanted something not overused but still holding industry weight |
| 11 | Dashboard "story" — Analyst view vs. Business view? | Both |
| 12 | Streamlit input — CSV batch, manual entry, or both? | CSV batch upload |

**Key discovery-phase decision:** the dataset. I wanted something with real industry credibility that wasn't the overused Kaggle credit card dataset. Claude compared three options — PaySim, Bank Account Fraud (NeurIPS 2022, feedzai), and IEEE-CIS — on recognition, realism, and build complexity. We locked in **Bank Account Fraud (NeurIPS 2022)** for its stronger industry credibility (published via feedzai, a real fraud-detection company, through a peer-reviewed NeurIPS competition track).

**Key scoping decision:** I wanted both an Analyst view and a Business view without doubling the workload. Instead of building two dashboards that each try to do everything, we split the two views across the two tools by what each is naturally best at — Streamlit for prediction/analyst work, Power BI for trends/business reporting. Same model, same dataset, no duplicated effort.

---

## ✅ Approved Project Summary

> We're building **FraudLens**, a two-part fraud detection product using the Bank Account Fraud (NeurIPS 2022, feedzai) dataset's "Base" variant. A Python-trained classification model (beginner-friendly algorithms like Logistic Regression/Random Forest, plus a basic anomaly detection technique) will power two connected views: a **Streamlit web app** (deployed live via Streamlit Community Cloud) where a user uploads a CSV of transactions and receives fraud predictions with explanations — the "Fraud Analyst" view — and a **Power BI report** (published live to Power BI service) showing fraud trends, volume, and patterns — the "Risk Manager/Business" view. Both views share the same underlying model and dataset, so the two tools each do what they're naturally best at without duplicating effort. The build is intentionally scoped tight to fit a 1–2 hour/day pace over 9 remaining days: one dataset, one model, one Streamlit app, one Power BI report — no multi-model comparisons, no fairness/bias research angle (despite the dataset supporting it), and no advanced deployment infra. Success on Day 10 means both the Streamlit app and Power BI report are live on the web, publicly viewable via shareable links, backed by a working trained model, and fully documented on GitHub/LinkedIn as a polished, demo-able v1.0 product.

---

## 📄 Deliverable 1 — Product Requirements Document (PRD)

**File:** `FraudLens_PRD.docx`

**Contents:**
1. Overview
2. Problem Statement
3. Target Users — Fraud Analyst persona, Risk Manager/Business persona
4. Goals & Success Metrics
5. Scope — v1.0 (In Scope): one dataset, one model pipeline (+ Isolation Forest secondary check), Streamlit app with batch upload/predictions/explanations, Power BI report with 4-6 visuals + DAX measure, free-tier deployment, full documentation
6. Explicitly Out of Scope: multi-model comparison, fairness/bias research, auth/database, real-time streaming, SHAP/LIME, mobile app, paid tools
7. Functional Requirements table (FR-1 through FR-9)
8. Non-Functional Requirements — usability, cost, maintainability, documentation
9. Data Source — Bank Account Fraud Dataset (NeurIPS 2022, feedzai)
10. Success Criteria — Day 10 Definition of Done
11. Risks & Mitigations table
12. Future Scope (Post-v1.0)

---

## 🗺 Deliverable 2 — Implementation Blueprint (Days 2–10)

**File:** `FraudLens_Implementation_Blueprint.md`

This is the single source of truth for the rest of the build. Each day includes: objective, what I'll learn, features to build, step-by-step plan, files/folders, tools/APIs, testing tasks, common issues, end-of-day checklist, expected screenshots, and handoff notes to the next day.

| Day | Focus |
|---|---|
| 2 | Environment setup + dataset acquisition + first-pass exploration |
| 3 | Data cleaning/encoding + Power BI account setup |
| 4 | Train/test split + baseline Logistic Regression model |
| 5 | Random Forest improvement + Isolation Forest comparison + save final model |
| 6 | Streamlit app core build — CSV upload + predictions |
| 7 | Streamlit app polish — charts, top-suspicious view, plain-language explanations |
| 8 | Deploy Streamlit live via GitHub + Streamlit Community Cloud; start Power BI report |
| 9 | Finish Power BI visuals + DAX measure + publish live to web |
| 10 | Final integration testing, README, screenshots, LinkedIn post, documentation, wrap-up |

Also includes: a pastable "project-wide context" block (for starting any day in a fresh AI conversation), a v1.0 exclusions list, and the full project folder structure.

---

## 🎯 Deliverable 3 — Project Pitch Deck

**File:** `FraudLens_Pitch_Deck.pptx`

**Slides:**
1. **Title** — FraudLens: catching fraud where it happens, for analysts and decision-makers alike
2. **The Problem** — fraud hides in volume (<0.5% typical fraud rate); two teams with two different needs; cost of getting it wrong
3. **Target Users** — The Fraud Analyst (fast scoring, ranked flags, plain-language reasons) vs. The Risk Manager (volume, trends, patterns, reporting-ready)
4. **The Solution** — one model, two views: Streamlit (Fraud Analyst view) and Power BI (Risk Manager view)
5. **Key Features** — batch prediction, explainable flags, anomaly cross-check, live trend reporting, fully deployed, zero cost
6. **Technical Approach** — data → model → app → report → deploy, in five numbered steps
7. **Future Scope & Vision** — SHAP/LIME explainability, fairness study, real-time API scoring, user accounts; vision statement on serving both technical depth and business clarity from one shared model

---

## 🧠 Key Learnings from Day 1

- **Scoping is a founder skill, not just a dev skill.** The hardest decision of the day wasn't choosing a project — it was deciding what two tools should each own, instead of trying to make one do everything.
- **"Both" doesn't have to mean double the work.** When two audiences seem to need two different products, check whether their needs map naturally onto two existing tools' strengths before assuming you need to build twice.
- **Dataset choice is itself a scoping decision.** A dataset with more industry credibility (NeurIPS-benchmarked, created by a real fraud-detection company) was worth trading off some simplicity for, since it directly affects how the finished project reads to a recruiter.
- **Time budget should shape scope from minute one, not get discovered as a constraint later.** Locking "1-2 hrs/day" early meant every later decision (model complexity, deployment approach, feature count) could be checked against it immediately instead of causing a crunch near Day 9-10.
- **A blueprint should let a fresh AI conversation continue the work with zero re-explaining.** Structuring each day with full context, files, and handoff notes protects the whole 10-day arc from losing continuity partway through.

---

## 🔗 Profile Links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
