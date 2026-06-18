# Day 18 — Brain Dump Action Planner | #60DayClaudeChallenge

> **Challenge:** #60DayClaudeChallenge &nbsp;|&nbsp; **Day:** 18 of 60  
> **Builder:** Urooj Fatima ; **Date:** June 18, 2026  
> **Tags:** `#ClaudeAI` `#AIProductivity` `#PromptEngineering` `#BuildInPublic` `#SkillBuilding`

---

## 📌 Overview

On Day 18, I built a **custom Claude skill** called the **Brain Dump Action Planner** — a structured AI workflow that transforms any messy, unstructured input (notes, brain dumps, voice memos, meeting transcripts) into a polished, interactive HTML project dashboard.

The key principle: **Claude structures only what you give it. It never invents, infers, or fills in the gaps.** Every missing field is explicitly surfaced as `Not specified`.

---

## 🧠 What Is a Claude Skill?

A **Claude Skill** is a reusable instruction file (`.skill` / `SKILL.md`) that teaches Claude a consistent workflow. Instead of re-prompting from scratch every session, you define the behavior once — modes, output format, rules, design system — and Claude executes it reliably.

Skills live in `/mnt/skills/` and are triggered by the `/skill-name` command.

---

## 🛠️ Custom Skill — `brain-dump-action-planner`

### Skill Definition

```yaml
name: brain-dump-action-planner
description: >
  Transform messy notes, meeting transcripts, voice memos, brainstorming
  sessions, and stream-of-consciousness thoughts into structured summaries,
  action plans, decisions, open questions, and task lists — presented as a
  beautiful interactive HTML dashboard.

  Triggers:
  - User pastes raw notes, a transcript, or a brain dump
  - "organize my notes", "turn this into action items", "clean up this meeting"
  - "what do I need to do from this", "make sense of this"
  - Even for short or partial inputs — always produce the full HTML dashboard

  Never use markdown output. Always generate the self-contained HTML artifact.
```

---

### Three Operating Modes

The skill auto-detects the correct mode from the user's input:

| Mode | Trigger | Extra Sections |
|------|---------|----------------|
| **Full Breakdown** | Default — any unstructured notes or brain dump | — |
| **Transcript Mode** | Speaker labels detected (`John:`, `[Sarah]`, `Speaker 1:`) | Speaker Summary, Decisions by Speaker, Action Items by Speaker, Attribution Notes |
| **Merge Mode** | 2+ separate source inputs provided | Source Information, Duplicate Items, Conflict Resolution Review |

---

### Core Rules (Non-Negotiable)

```
1. Never invent, infer, or assume — output only what is explicitly stated
2. Preserve exactly: all names, dates, numbers, deadlines, terminology, speaker labels
3. Missing information → display "Not specified" (never guess or fill in)
4. Never resolve conflicts automatically — flag them for human review
5. Output is always a complete self-contained HTML artifact starting with <style>
```

---

### Required Output Sections (All Modes)

| # | Section | Display Format |
|---|---------|---------------|
| 1 | **Summary** | 2–5 sentence prose overview |
| 2 | **Key Takeaways** | Responsive card grid (3-col) |
| 3 | **Action Items** | Interactive table — Task / Owner / Deadline / Status |
| 4 | **Open Questions** | List with ❓ badge |
| 5 | **Risks / Blockers** | List with priority badges |
| 6 | **Conflicts** | Flagged items with ⚠️ — never auto-resolved |
| 7 | **Additional Notes** | Supporting context that fits nowhere else |

---

### Status Badge System

```
🔴 High Priority   → #fee2e2 / #9b1c1c  — Urgent tasks, critical blockers
🟠 Medium Priority → #ffedd5 / #9a3412  — Standard action items
🟢 Low Priority    → #dcfce7 / #14532d  — Optional, nice-to-haves
⚠️  Conflict        → #fef9c3 / #713f12  — Contradictions, unclear ownership
❓ Open Question   → #ede9fe / #4c1d95  — Unresolved topics
✅ Completed       → #dcfce7 / #14532d  — Done items
⏳ Pending         → #f3f4f6 / #4b5563  — Awaiting action
```

---

### Design System

```
Dashboard aesthetic: Notion + Linear + ClickUp
Font:       Inter (Google Fonts) / system-ui fallback
Background: #f8f9fc
Card bg:    #ffffff
Primary:    #5b6af0
Border:     #e5e7eb
Text:       #111827 (primary) / #6b7280 (muted)
Features:   Collapsible sections, hover states, responsive grid, badge pills
```

![Skill Design System – Badge & Color Preview](<img width="1112" height="737" alt="day18-skill-design-system png" src="https://github.com/user-attachments/assets/2c67e640-6a83-4f31-beaf-a5b7af1fbd21" />
)

---

### HTML Output Template

```html
<style>
  /* Full CSS — Inter font, color tokens, layout, cards,
     tables, badges, collapsibles, responsive breakpoints */
</style>

<!-- Header: title + mode badge + timestamp -->
<header>...</header>

<main>
  <section id="summary">...</section>
  <section id="takeaways">...</section>
  <section id="action-items">...</section>
  <section id="open-questions">...</section>
  <section id="risks">...</section>
  <section id="conflicts">...</section>
  <section id="notes">...</section>
  <!-- Transcript/Merge extra sections injected here -->
</main>

<script>
  /* Toggle logic for collapsible sections */
  function tog(hdr) {
    const b = hdr.nextElementSibling;
    const btn = hdr.querySelector('.toggle');
    const open = b.style.maxHeight !== '0px' && b.style.maxHeight !== '';
    b.style.maxHeight = open ? '0px' : b.scrollHeight + 'px';
    btn.textContent = open ? '+' : '−';
  }
  document.querySelectorAll('.sec-body')
    .forEach(b => b.style.maxHeight = b.scrollHeight + 'px');
</script>
```

---

## 📊 Generated Dashboards

### Dashboard 1 — Skill Test (Product Launch Planning)

**Input type:** Simulated product launch brain dump  
**Mode triggered:** Full Breakdown  
**Input summary:** Planning notes for a Q3 product launch involving engineering, design, marketing, legal, and a vendor contract.

**Structured Output:**

| Section | Items Extracted |
|---------|----------------|
| Key Takeaways | 6 cards (launch target, pricing, legal, vendor, design, marketing vs. engineering conflict) |
| Action Items | 7 tasks with owners, deadlines, and status badges |
| Open Questions | 5 unresolved items flagged |
| Risks / Blockers | 5 risks across High / Medium / Low priority |
| Conflicts | 2 conflicts flagged (runway disagreement, pricing ownership) |
| Additional Notes | 3 supporting context items |

**Notable output behavior:**
- Marketing vs Engineering conflict (6-week vs 4-week runway) was surfaced as ⚠️ Conflict without resolution
- All `Not specified` fields were rendered in muted italic — no fabricated data


---

### Dashboard 2 — Personal Task List (Real Input)

**Input (verbatim):**
```
do projects before deadline, deadline still not provided
do stats
research work topics to prepare
deploy the projects on github
```

**Mode triggered:** Full Breakdown  
**Processing time:** Seconds

**Structured Output:**

| Section | Content |
|---------|---------|
| Summary | 4-task personal plan across academic and technical work; no owners or deadlines specified |
| Key Takeaways | 4 cards (projects, stats, research, GitHub deployment) |
| Action Items | 4 rows — all owners and deadlines: `Not specified` |
| Open Questions | 5 questions (deadline, which projects, what stats, which topics, all vs. specific repos) |
| Risks / Blockers | 🔴 Missing deadline / 🟠 Undefined research scope / 🟠 Undefined project list |
| Conflicts | None identified |
| Additional Notes | Deadline explicitly acknowledged as "still not provided" in the original notes |

**Key behavior demonstrated:**
> The dashboard surfaced the missing deadline as a 🔴 High Priority blocker — making a known-but-ignored gap impossible to dismiss.

![Dashboard 2 – Personal Task List](day18-dashboard-personal-tasks.png)

---

## 💡 Key Learnings

### 1. Skill files create reproducible AI behavior
Writing a `SKILL.md` is like writing a spec for a teammate. Once defined, Claude executes the same structured workflow every time — no re-prompting, no drift, no inconsistency across sessions.

### 2. "Not specified" is a feature, not a limitation
Forcing the output to surface missing information (rather than filling it in) is the most practically valuable design decision. It makes gaps actionable.

### 3. Conflict detection without resolution is the right default
Auto-resolving conflicts would create a false sense of clarity. Flagging them with ⚠️ and leaving them for human review is more honest and more useful.

### 4. Even a 4-line brain dump produces structured value
The second dashboard (4 lines, no deadlines, no owners) generated 5 open questions and 3 prioritized blockers. Short inputs still yield structured, actionable outputs.

### 5. Mode auto-detection reduces friction
Detecting Transcript vs. Merge vs. Full Breakdown automatically means the user never has to think about mode selection — they just paste their input.

### 6. Dashboard design affects behavior
A 🔴 High Priority badge on a clean dashboard is harder to ignore than a bullet point in a text list. Visual hierarchy changes what gets acted on.

---

## 🔁 Workflow Summary

```
Raw messy input (notes / transcript / brain dump)
        ↓
/brain-dump-action-planner  [skill trigger]
        ↓
Mode detected automatically (Full / Transcript / Merge)
        ↓
Claude extracts: summary → takeaways → action items →
                 open questions → risks → conflicts → notes
        ↓
Interactive HTML dashboard rendered inline
        ↓
Collapsible sections + color-coded badges + hover tables
        ↓
Human reviews gaps flagged as "Not specified" / ⚠️ Conflicts
        ↓
Clarity achieved — ready to act
```

---

## 📁 Files & Resources

| File | Description |
|------|-------------|
| `brain-dump-action-planner.skill` | Packaged Claude skill file |
| `SKILL.md` | Skill instruction source |
| `day18.md` | This documentation file |

---

## 🔗 Connect

| Platform | Link |
|----------|------|
| LinkedIn | [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey) |
| GitHub | [github.com/uroojey](https://github.com/uroojey) |
| Portfolio | [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app) |



> *Every day, one build. Every build, one learning. Every learning, one step forward.*
