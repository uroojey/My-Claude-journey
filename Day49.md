# 🧠 Personal AI Playbook

![Day](https://img.shields.io/badge/60DayClaudeChallenge-Day%2049-0891b2?style=flat-square)
![Status](https://img.shields.io/badge/status-shipped-34d399?style=flat-square)
![Stack](https://img.shields.io/badge/stack-HTML%20%2F%20CSS%20%2F%20JS-0891b2?style=flat-square)
![Dependencies](https://img.shields.io/badge/dependencies-none-blue?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-lightgrey?style=flat-square)

A self-contained, single-file web app that turns everyday AI habits into **reusable workflow systems** instead of one-off prompts rewritten from scratch every time. Built as Day 49 of the [#60DayClaudeChallenge](https://www.linkedin.com/in/uroojey).

> Not a prompt library. A prompt **system**.

---

## 🔗 Live file

The entire app lives in one file: **[`personal-ai-playbook.html`](./personal-ai-playbook.html)**. Download it and open it in any browser — no build step, no server, no install.

---

## 🧩 What it does

| Section | Purpose |
|---|---|
| **Dashboard** | Library stats, the 3-stage daily pipeline (Build → Post → Document), and workflows recommended for your role |
| **My Workflows** | Saved, categorized, ready-to-copy prompt templates — search, filter, favorite, duplicate, edit, delete |
| **Prompt Builder** | Assemble a prompt from 9 labeled building blocks (role, objective, context, constraints, reasoning strategy, output format, tone, examples, quality checks) with a live preview, instead of writing paragraphs by hand |
| **Loop Builder** | Turn any one-shot prompt into an autonomous loop with a goal, evaluation criteria, an improvement strategy, stop conditions, and safety rules |
| **Settings** | Theme toggle, keyboard shortcuts, import/export your entire library as JSON |

Every block in the Prompt Builder and Loop Builder carries a plain-language explanation both in the picker and once it's added, so nothing requires guessing what "reasoning strategy" or "stop conditions" means.

---

## 📦 Project structure

```
personal-ai-playbook.html         # the entire app — HTML, CSS, JS in one file
ai-playbook-workflows-export.json # the 9 default workflows, exportable/importable from the Settings tab
README.md                         # this file
day49.md                          # daily build documentation + LinkedIn variants
```

---

## ⚙️ Features & tech

| Feature | Implementation |
|---|---|
| Zero dependencies | Vanilla HTML/CSS/JS only, fully offline-capable |
| Persistence | `localStorage`, no backend |
| Prompt Builder | 9 modular building blocks combine into effectively unlimited prompt variations |
| Loop Builder | Converts any prompt into a self-evaluating, self-stopping loop |
| Workflow library | 9 pre-built workflows across 5 categories (Coding & Debugging, Content & Documentation, Data Analysis, Career Development, Prompt Engineering) |
| Import / export | One-click JSON export/import of the full workflow library |
| Theming | Dark/light mode toggle, CSS custom properties |
| Onboarding | First-run modal + persistent "What is this?" help affordance, always available |
| Keyboard shortcuts | `1`–`4` to jump sections, `N` new workflow, `/` focus search, `Esc` close modals |
| Responsive | Down to mobile breakpoints |

---

## ▶️ How to run

1. Download `personal-ai-playbook.html`.
2. Open it directly in any modern browser (Chrome, Edge, Firefox, Safari).
3. That's it — no npm install, no server, no API key required.

To load the pre-built workflow set on a fresh browser profile, go to **Settings → Import library** and select `ai-playbook-workflows-export.json`.

---

## 🧪 Testing methodology

- `node --check` on the extracted `<script>` block — zero syntax errors
- Playwright (headless Chromium) smoke test covering: onboarding dismissal, navigation across all 5 views, Prompt Builder block add/preview flow, Loop Builder field entry, theme toggle, and workflow favoriting — zero console errors
- Manual pass for empty states, search/filter combinations, and import/export round-trip

---

## 💡 Key learnings

1. **The bottleneck was never the AI, it was the missing system.** Rewriting prompts from scratch every day and calling it "customizing" was really just repeated, uncaptured effort. Turning recurring patterns into named, editable blocks is what actually saves time.
2. **Explaining the "why" beats hiding it.** Adding a short explanation to every building block, in both the picker and the assembled view, matters more for real usability than clever labels or novel terminology.
3. **A loop needs a definition of "better," not just an instruction to improve.** Evaluation criteria and stop conditions are what make an autonomous loop safe and finite instead of an open-ended retry.
4. **Modular blocks beat a big prompt library.** Nine reusable building blocks can combine into far more variations than a static list of finished prompts ever could, and they stay useful as the underlying task changes.

---

## 🔗 Profile links

- LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- GitHub: [github.com/uroojey](https://github.com/uroojey)
- Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- Email: uroojfatima4111@gmail.com

---

Built as part of the **#60DayClaudeChallenge** with [@Anthropic](https://www.anthropic.com) · [@ABTalksOnAI](https://linkedin.com/in/anilbajpai) · [@AnilBajpai](https://linkedin.com/in/anilbajpai)
