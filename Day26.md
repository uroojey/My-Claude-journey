# Day 26 — Prior Authorization Workflow Simulator

**Challenge:** #60DayClaudeChallenge
**Builder:** Urooj Fatima
**Tool:** Claude (Anthropic)
**Category:** Healthcare AI · Workflow Design · Gamified Simulation

---

## What I Built

A fully interactive, gamified **Prior Authorization (PA) Workflow Simulator** — single HTML file, zero external dependencies, vanilla JavaScript only.

The simulator walks users through the real US healthcare PA process across three swimlane columns: **Patient → Provider → Payer**, with drag-and-drop case movement, decision branching, and educational tooltips at every stage.

---

## Generated HTML File

**File:** `pa-workflow-simulator.html`
**Size:** ~57 KB
**Tech:** HTML + CSS + Vanilla JS (no frameworks, no CDN, no build step)

### Features at a Glance

| Feature | Details |
|---|---|
| Patient Scenarios | Elective Knee Replacement, Brain MRI, Specialty Biologic Medication, Inpatient Psychiatric Admission |
| Workflow Lanes | Patient · Provider · Payer |
| Stages | Intake → Referral → Medical Necessity → Document Collection → Submission → Payer Review → Decision → Outcome |
| Interactions | Drag-and-drop case chips, click fallback, document checklist, decision buttons |
| Outcomes | Approve · Pend · Deny · Appeal · Peer-to-Peer Review |
| Gamification | Efficiency score (0–100), days elapsed counter, animated SVG score ring, confetti on approval |
| Education | Context-aware explanation card updates at every workflow step |
| Summary | Full activity timeline log + workflow summary modal on completion |
| State | All managed in JS memory — no localStorage, no backend |

### Scenario Data Structure (editable array)

```javascript
const SCENARIOS = [
  {
    id: 'elective-surgery',
    name: 'Elective Knee Replacement',
    icon: '🦴',
    badge: 'Elective Surgery',
    desc: 'Patient John M., 58, needs a right total knee arthroplasty...',
    patient: { name, age, insurance, plan, diagnosis, requested },
    documents: [ /* checklist items */ ],
    medNecessityScore: 78,
    baseApprovalOdds: 0.70,
    appealable: true,
    urgency: 'Routine',
  },
  // ... 3 more scenarios
];
```

### Workflow Steps

```javascript
const STEPS = [
  { id: 'intake',       label: 'Intake',         lane: 'patient'  },
  { id: 'referral',     label: 'Referral',        lane: 'provider' },
  { id: 'med-nec',      label: 'Med Necessity',   lane: 'provider' },
  { id: 'doc-collect',  label: 'Docs',            lane: 'provider' },
  { id: 'submission',   label: 'Submit',          lane: 'payer'    },
  { id: 'review',       label: 'Review',          lane: 'payer'    },
  { id: 'decision',     label: 'Decision',        lane: 'payer'    },
  { id: 'outcome',      label: 'Outcome',         lane: 'patient'  },
];
```

---

## Completed Workflow Summary

### Sample Run — Inpatient Psychiatric Admission (David L., 29)

| Metric | Value |
|---|---|
| Scenario | Inpatient Psychiatric Admission |
| Insurance | Medicaid Managed Care — Basic Coverage |
| Diagnosis | Severe MDD with suicidal ideation (F32.2) |
| Requested Service | Inpatient Psychiatric Admission (DRG 885) |
| Medical Necessity Score | 91 / 100 |
| Documents Required | 4 |
| Documents Submitted | 4 / 4 (complete) |
| Urgency | Emergent |
| Days Elapsed | 6 |
| Pend Count | 0 |
| Appeal Used | No |
| P2P Review Used | No |
| Final Outcome | ✅ Approved |
| Efficiency Score | 100 / 100 (Grade: A) |

### Activity Log

| Day | Event | Detail |
|---|---|---|
| 0 | Patient Intake | Case initiated. Insurance eligibility verified. PA required. |
| 1 | Referral & Clinical Order | Ordering physician documented service and ICD-10 code. |
| 2 | Medical Necessity Confirmed | Score: 91/100 — Strong justification. |
| 4 | Complete Document Package Submitted | All 4 documents collected and verified. |
| 5 | PA Received by Payer | PA reference number assigned. SLA: 24 hrs (Emergent). |
| 5 | Payer Clinical Review | Nurse reviewer checked criteria — all documents present. |
| 6 | PA Approved | Authorization issued. Service may proceed. Auth valid: 90 days. |

### Sample Run — Specialty Biologic (Aisha R., 42) — With Denial & Appeal

| Metric | Value |
|---|---|
| Scenario | Specialty Biologic Medication (Adalimumab) |
| Medical Necessity Score | 82 / 100 |
| Documents Submitted | 3 / 5 (incomplete — submitted anyway) |
| Days Elapsed | 18 |
| Pend Count | 1 |
| Appeal Used | Yes |
| P2P Review Used | Yes |
| Final Outcome | ✅ Approved (post-appeal) |
| Efficiency Score | 55 / 100 (Grade: C) |

**Key observation from this run:** Incomplete document submission triggered a pend (+2 days, −10 score). Initial denial required a first-level appeal (+5 days, −15 score). P2P review (+1 day) boosted approval probability by 10% and ultimately contributed to the appeal overturn.

---

## Key Learnings

### 1. The PA Workflow Is a Documentation Game, Not a Medical Game
Medical necessity matters — but it is not sufficient. A score of 91/100 with missing documents will pend. A score of 65/100 with complete, well-organized documentation will often approve. The system rewards documentation discipline as much as clinical appropriateness.

### 2. The "Pend" Outcome Is the Hidden Cost Driver
A pend feels like a minor delay but compounds quickly: each pend adds 2+ days to the timeline, reduces efficiency, and requires re-submission. In real-world PA, pends account for a significant portion of administrative burden and are often caused by incomplete initial submissions — a preventable inefficiency.

### 3. Peer-to-Peer Review Is Underutilized
Modeling P2P review as a mechanic (with a +10% approval probability boost) made clear how powerful this pathway is. A direct physician-to-physician conversation can overturn denials that documentation alone could not. Yet many providers either don't request it or don't know it's available in time.

### 4. Urgency Changes Everything — But Only If Flagged Correctly
Emergent cases have 24-hour SLAs; routine cases can take 5–7 business days. In the simulator, urgency classification directly affects payer response time. Miscategorizing an urgent case as routine is a workflow failure that delays care — and it happens in real systems.

### 5. Drag-and-Drop as a Metaphor for Real Handoffs
Every drag-and-drop interaction in the simulator represents a real handoff — patient to provider, provider to payer, payer back to provider. Designing the drag interaction made visible how many touch points exist in a single authorization. Each handoff is a potential point of delay, error, or information loss.

### 6. Workflow Design Principle: Surface Decision Pathways Early
Appeal and P2P options exist but are buried late in the PA flow. A better-designed system would surface these options earlier — ideally at the first sign of clinical complexity — rather than only after a denial. This is a general workflow design lesson: exception paths should be visible, not hidden.

### 7. Building a Simulation Forces You to Understand the System
You cannot simulate a workflow you don't understand. Mapping every stage, branch condition, and outcome into code required internalizing the PA process at a structural level. The build process itself was the learning process.

---

## Technical Notes

### Bugs Fixed During Build
| Bug | Cause | Fix |
|---|---|---|
| `Unexpected identifier 's'` | Curly apostrophe (`'`) in `Crohn's` inside a single-quoted JS string | Switched wrapping string to double quotes |
| `restartGame is not defined` | Cascade failure — JS crashed before `window.restartGame` was defined | Fixed by resolving upstream syntax errors |
| Nested template literal syntax error | Backtick inside backtick in `.map(d=>\`✓ ${d}\`)` | Replaced with `.map(function(d){ return '&#10003; ' + d; })` |

### Lessons for Single-File HTML Builds
- Curly/smart apostrophes (`'`) inside single-quoted JS strings are silent killers — always use straight quotes
- Nested template literals (backtick inside backtick) are invalid without tagged template support — use string concatenation instead
- When a `window.X is not defined` error appears on a globally defined function, look upstream for a crash that prevented the script from finishing execution

---
## Links
- 💼 LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- 🌐 Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
