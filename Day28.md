# Day 28 — Hospital Admission Readiness Simulator
**#60DayClaudeChallenge · Built with Claude (claude.ai)**

---

## 📌 What I Built

A single-file interactive **Hospital Admission Readiness Simulator** where the user plays the role of a Hospital Admission Coordinator.

The simulator walks through a complete admission workflow — from case setup to final admit/not-ready decision — scoring readiness across six weighted clinical and administrative components in real time.

> **Disclaimer:** All provider names, payer names, and data in this simulator are illustrative training data only. This tool is not intended for clinical use.

---

## 🛠 Tech Stack

| Layer | Choice |
|---|---|
| Markup | HTML5 (single file) |
| Styling | Tailwind CSS (CDN) + custom CSS variables |
| Logic | Vanilla JavaScript |
| Design | Dark navy glassmorphism — `#060d1a` base, `#0891b2` teal accent |
| Font | Inter (UI) + JetBrains Mono (scores/data) |

---

## 🖥 App Structure

### Phase 1 — Case Setup
User completes six fields before any analysis begins:

- **Provider / Facility** — illustrative training hospitals
- **Attending Physician** — specialty-labelled illustrative physicians
- **Primary Diagnosis** — Acute MI / CHF / Pneumonia / Elective Surgery / Hip Fracture
- **Admission Type** — Inpatient / Observation / Emergency / ICU / Same-Day Surgery
- **PA Status** — Approved / Pending / Denied
- **Insurance / Payer** — Commercial / Medicare / Medicaid / Managed Care (training labels)
- **Admission Date**

> **Observation Status** always triggers:
> *"CMS 2-Midnight Rule applies — different cost-sharing, SNF eligibility, and billing than inpatient. Medicare patients require written MOON notification."*

> **AMI / CHF diagnosis** always triggers:
> *"InterQual/Milliman thresholds apply — ensure documentation meets medical necessity standards before UR review."*

---

### Phase 2 — Initial Analysis (Score: 30–60%)

On clicking **🏥 Analyze Admission Readiness**, the system generates a status panel for all six components:

| Component | Weight | Description |
|---|---|---|
| PA Status | 25% | Authorization state drives the ceiling |
| Clinical Documentation | 20% | Baseline lower for AMI/CHF |
| Physician Orders | 20% | Starts incomplete until actions taken |
| Insurance Verification | 15% | Varies by payer type |
| Consent | 10% | Incomplete until workflow action |
| Bed Assignment | 10% | Lower baseline for ICU |

**Score range at analysis:** 30–60% — final decision is not revealed.

---

### Phase 3 — PA Branch Workflow

PA Status drives three distinct branches:

**✅ Approved**
- Continue with standard admission workflow.

**⏳ Pending**
- 📞 Follow Up with Payer → +10% PA, small order boost
- 📤 Upload Supporting Docs → +15% PA, +10% Documentation
- 👩‍⚕️ Contact Attending Physician → +8% PA, +10% Orders

**❌ Denied**
- 🔍 Review Denial Reason → no score change; surfaces UR context
- 📞 Contact Insurance → +5% PA
- 📨 Submit Appeal → flags appeal as submitted
- 🎲 Simulate Appeal Outcome → 65% chance of approval
  - **Approved:** PA converts to Approved, score unlocks
  - **Denied:** Ceiling remains; peer-to-peer review suggested

> **Hard constraint:** Denied PA + ICU admission cannot exceed **62% readiness** regardless of admin tasks completed.

---

### Phase 4 — Workflow Actions

Seven actions that each boost specific component scores:

| Action | Component Boost |
|---|---|
| 🛏 Assign Bed | Bed +30% |
| 💳 Verify Insurance | Insurance +25% |
| 📋 Upload Documentation | Documentation +30% |
| ✍ Complete Consent | Consent +55% |
| 👩‍⚕️ Contact Physician | Orders +50% |
| 🔔 Notify Nursing | Orders +15% |
| 🚑 Prepare Patient Arrival | Bed +15% |

All completed actions are timestamped in an activity log.

---

### Phase 5 — Supporting Panels

**📅 Admission Timeline**
Nine milestones tracked visually with completion state:
PA Review → Insurance Verification → Bed Assignment → Documentation → Consent → Patient Arrival → Registration → Clinical Assessment → Admission Complete

**👥 Care Coordination Cards**
Five team roles with responsibilities:
- Attending Physician
- Case Manager
- Nursing
- Utilization Review *(concurrent review · denial risk identification · InterQual · Milliman)*
- Discharge Planner

**⚠ Risk Tracking**
Four risk categories, each with a severity bar:

| Risk | Elevated When |
|---|---|
| Documentation Risk | Score < 60%; higher weight for AMI/CHF |
| Insurance Risk | Score < 65% |
| Bed Risk | ICU + unassigned |
| Clinical Risk | **Weighted higher** for AMI, CHF, ICU admissions |

---

### Phase 6 — Governance Snapshot (≥ 75%)

Unlocks when readiness reaches 75%:

> *"Industry benchmarks (estimates only): PA turnaround 3–5 days · Inpatient denial rate ~8–10% (CMS) · PA rework cost ~$11/transaction (CAQH)"*

---

### Phase 7 — Final Decision

| Score | Outcome |
|---|---|
| **≥ 90%** | ✅ **ADMIT** — full summary with all component statuses |
| **< 90%** | ⚠ **NOT READY** — missing items listed, required actions highlighted |

---

## 🏥 Completed Admission Scenarios

### Scenario 1 — Straightforward Admit
**Setup:** Acute MI · Inpatient · PA Approved · Medicare
**Path:** All 7 workflow actions completed in sequence
**Outcome:** Score reached 93% → ✅ Admitted
**Note:** InterQual/Milliman banner triggered on setup; documentation risk started High, resolved after upload action

---

### Scenario 2 — PA Pending, Successfully Resolved
**Setup:** CHF · Inpatient · PA Pending · Commercial
**Path:** Followed all three PA pending actions (follow-up, docs, physician), then completed workflow
**Outcome:** Score climbed from 38% → 91% → ✅ Admitted
**Note:** Pending PA resolved through simulated payer engagement; documentation + order components boosted via physician contact

---

### Scenario 3 — Denied PA, Successful Appeal
**Setup:** Pneumonia · Inpatient · PA Denied · Managed Care
**Path:** Reviewed denial → contacted insurance → submitted appeal → appeal approved (simulated)
**Outcome:** PA converted to Approved; completed remaining actions → 90% → ✅ Admitted
**Note:** Before appeal, score was capped below 65%; appeal unlock was required to proceed

---

### Scenario 4 — Denied PA + ICU Hard Cap
**Setup:** Acute MI · ICU · PA Denied · Medicare
**Path:** All 7 workflow actions completed; appeal submitted but denied
**Outcome:** Score reached 62% — hard cap enforced → ⚠ Not Ready
**Note:** ICU + Denied PA ceiling is a real operational constraint encoded in the simulator; no amount of admin completion overcomes it. Peer-to-peer review with medical director was the recommended path forward.

---

### Scenario 5 — Observation Status Notice Triggered
**Setup:** Elective Surgery · Observation · PA Approved · Medicare
**Path:** Standard workflow completed
**Outcome:** 88% → ⚠ Not Ready (consent incomplete)
**Note:** CMS 2-Midnight Rule banner appeared immediately on admission type selection; MOON notification requirement highlighted for Medicare patient

---

## 💡 Key Learnings

### 1. Prior Authorization is the structural ceiling, not a checkbox
PA Status carries 25% of the readiness weight — the highest of any single component. A denied PA doesn't just reduce the score, it actively caps it. For ICU admissions with denied PA, the system cannot exceed 62% regardless of every other action being complete. This reflects a real operational truth: clinical and administrative completeness cannot substitute for authorization.

### 2. Observation ≠ Inpatient — and the difference costs patients money
The CMS 2-Midnight Rule makes Observation status a fundamentally different classification from Inpatient — not just in billing, but in SNF eligibility and cost-sharing for the patient. Medicare patients under Observation status must receive a written MOON (Medicare Outpatient Observation Notice). This distinction is easy to overlook and expensive to miss.

### 3. InterQual and Milliman are documentation standards, not payer opinions
For AMI and CHF admissions, medical necessity isn't self-evident from the diagnosis — it must be documented to specific thresholds before Utilization Review. InterQual and Milliman criteria are the benchmarks UR uses during concurrent review. Poor documentation creates denial risk that no appeal fully recovers from cleanly.

### 4. The Utilization Review function is a denial prevention role, not a gatekeeper
Building the UR care coordination card made this clear: UR's job is concurrent review and denial risk identification — catching gaps before the payer does, not after. Encoding this distinction into the simulator changed how I thought about the entire workflow sequence.

### 5. Simulation teaches operational logic that reading doesn't
Encoding the PA branch logic, the score weights, the hard cap rule, and the regulatory banners forced engagement with why each rule exists — not just what it says. The act of building the decision tree for each scenario made the operational constraints stick in a way that summarizing them never would have.

---

## 📁 File Reference

| File | Description |
|---|---|
| `day28.html` | Complete single-file simulator (HTML + Tailwind + Vanilla JS) |
| `day28.md` | This documentation file |

---

## 🔗 Links

- **GitHub:** [github.com/uroojey](https://github.com/uroojey)
- **Portfolio:** [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)
- **LinkedIn:** [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)

---
