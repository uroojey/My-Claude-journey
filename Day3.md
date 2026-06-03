_**Basic prompt without defining role:**_
Explain the basics of SQL to a complete beginner, increasing difficulty with different queries. 
Provide examples and real life usage of these queries.
Give interview questions asked for fresher roles that require SQL .

_**Output:**_
Here's your complete SQL guide in one interactive widget — click through each tab to explore:

**What is SQL?** — The core idea, the table structure, and the 4 fundamental operations (CRUD).

**Queries** — Goes from basic `SELECT` all the way through `WHERE`, `ORDER BY`, `LIMIT`,
aggregate functions (`COUNT`, `SUM`, `AVG`), `GROUP BY`, and `HAVING`, with real-life use cases at every step.

**JOINs** — The most important and most tested concept. Covers `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`,
and `FULL OUTER JOIN` with visual table examples so you can see what gets returned and what gets excluded.

**Advanced** — Subqueries, `DISTINCT`, `LIKE`, `IN`, window functions (`RANK() OVER`), and CTEs (`WITH` clause). 
These appear in analyst roles and distinguish a good fresher from a great one.

**Interview Qs** — 10 frequently asked questions for fresher/entry-level roles, covering both conceptual definitions and practical query writing.

A few things to keep in mind as you learn:

The order SQL executes clauses is different from how you write them: `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY` → `LIMIT`.
Understanding this order explains *why* you can't use a `WHERE` clause on an aggregate — the grouping hasn't happened yet at that stage.

For practice, HackerRank's SQL track and LeetCode's SQL section are excellent free resources with instant feedback. 
Mode Analytics also has a good beginner SQL tutorial if you prefer a narrative format.

**Optimized prompt defining developer persona:**_
You are an experienced SQL develepor. Explain SQL basics to a complete beginner and progressively increase difficulty with example queries. 
For each level, include: 1) A short explanation of the concept
2) 2–3 representative examples with real-life data
3) Common pitfalls and tips
4) Brief exercise prompts for practice
Then provide a practical overview of real-life usage scenarios (e.g., reporting, data cleaning, analytics)
and conclude with a curated list of common SQL interview questions for entry-level roles, along with concise sample answers or talking points.
Assume no prior programming experience. 
Use plain language, define key terms, and include at least: - SELECT, FROM, WHERE, GROUP BY, HAVING, ORDER BY
- Joins (INNER, LEFT JOIN)
- Aggregations (COUNT, SUM, AVG, MAX, MIN)
- Subqueries and basic set operations (UNION)
- Basic data types and simple data modeling concepts
- Format content with clear headings, short bullet points, and small runnable examples using a single hypothetical table
- (e.g., employees or sales) to illustrate each concept.

**Output:**
Here's your complete SQL Beginner's Guide as a polished Word document. 
https://docs.google.com/document/d/1hAh-LvLG49UtzxMd3VLloPok_welVXYCpkzIi73QpNI/edit?tab=t.0

Here's what's inside:
Structure at a glance:
The guide is organized across 8 progressive sections, all built around a consistent employees + departments dataset so every query is immediately relatable:
Level 1 — SELECT, FROM, WHERE — fetching data, filtering rows, AND/OR conditions, NULL handling, plus a breakdown of why WHERE city = NULL is always wrong.
Level 2 — ORDER BY, LIMIT, DISTINCT — sorting results, capping output, deduplicating with DISTINCT, and why LIMIT without ORDER BY is dangerous.
Level 3 — Aggregations, GROUP BY, HAVING — all five aggregate functions (COUNT, SUM, AVG, MAX, MIN) with the critical WHERE vs. HAVING distinction explained clearly, plus a table showing the actual execution order SQL follows.
Level 4 — JOINs — INNER JOIN, LEFT JOIN, RIGHT JOIN, FULL OUTER JOIN compared in a table; practical examples including how to use LEFT JOIN to catch orphan records for data cleaning.
Level 5 — Subqueries & UNION — scalar subqueries, inline views in FROM, correlated subqueries, and UNION vs. UNION ALL with a speed note.
Data Types & Modeling — a reference table of common types, primary/foreign keys, 1:N vs N:N relationships, normalisation in plain English, and a CREATE TABLE example.
Real-life usage scenarios — business reporting, data cleaning, analytics/KPI tracking, app backend queries, and ETL migration patterns — each with a runnable example.
15 interview Q&As — split into Conceptual, Query Writing, and Advanced sections, with full talking-point answers for each.
The document closes with a quick-reference cheat sheet covering clause execution order, common functions, and recommended practice resources (HackerRank, LeetCode, SQLZoo, Mode Analytics, DB Fiddle).
