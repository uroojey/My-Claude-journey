# Day 27 — Prior Authorization Story Simulator

**Challenge:** #60DayClaudeChallenge
**Builder:** Urooj Fatima
**Tool:** Claude (Anthropic)
**Category:** Healthcare AI · Storytelling · Interactive Education
**Continuation of:** Day 26 — PA Workflow Simulator

---

## What I Built

A character-driven, scene-by-scene **Prior Authorization Story Simulator** — built as a single HTML file with animated chat bubbles, branching choices, and educational context woven into every scene.

Where Day 26 showed the *system view* of PA (drag-and-drop workflow lanes, clinical scores, payer decisions), Day 27 shows the *human view* — what the process feels like for the patient living through it.

---

## Generated HTML File

**File:** `pa-story-simulator.html`
**Size:** ~36 KB
**Tech:** HTML + Tailwind CSS (CDN) + Vanilla JavaScript

> Note: The Tailwind CDN console warning (`cdn.tailwindcss.com should not be used in production`) is expected and harmless for a single-file demo. No action needed.

### Characters

| Character | Role | Bubble Style |
|---|---|---|
| 👦 Rahul | Patient, 28, newly diagnosed with Rheumatoid Arthritis | Left-aligned, white card, slate border |
| 👧 Priya | Healthcare Operations Specialist | Right-aligned, brand blue background, white text |
| Narrators | Scene context, statistics, system updates | Centered italic text only — never a bubble |
| System | PA status cards, reference numbers, decisions | Centered monospace card in brand-blue tint |

### Features at a Glance

| Feature | Details |
|---|---|
| Scenes | 8 complete scenes from diagnosis to approval |
| Chat Engine | `createElement + appendChild` only — `innerHTML` never called on chat container |
| Typing Indicator | Animated 3-dot indicator with character name before each bubble |
| Beat Timing | Per-beat delay based on text length — feels like real conversation |
| Branching Choices | 2 choices after every scene, staggered fade-in animation |
| Progress | Progress bar, scene dots, scene counter — all update live |
| Bubble Animation | CSS keyframe `bubbleIn` on every chat bubble |
| Narrator Style | Centered italic — visually distinct from character dialogue |
| System Cards | Mono-style PA status cards for approvals, denials, submissions |
| Restart | Full reset — clears chat, resets state, replays from scene 1 |

### Scene Overview

```
Scene 1 → Doctor Visit
Scene 2 → Insurance Roadblock
Scene 3 → What is Prior Authorization?
Scene 4 → Inside the Insurance Review
Scene 5 → The Denial
Scene 6 → Filing the Appeal
Scene 7 → Approval
Scene 8 → Takeaways
```

### Technical Architecture

```javascript
// Scene data structure
var SCENES = [
  {
    title: 'Scene 1: Doctor Visit',
    beats: [
      { who: 'narrator', text: '...', delay: 0   },
      { who: 'rahul',    text: '...', delay: 700  },
      { who: 'priya',    text: '...', delay: 800  },
      { who: 'system',   text: '...', delay: 600  },
    ],
    choices: [
      { label: '👆 Ask why insurance needs to approve it', scene: 1 },
      { label: '📋 Ask what happens next step by step',    scene: 1 },
    ]
  },
  // ...7 more scenes
];
```

```javascript
// Bubble factory — createElement only, never innerHTML on chat container
function makeBubble(who, text) {
  var wrapper = document.createElement('div');
  // Branches: narrator | system | rahul | priya
  // Each branch uses createElement + appendChild exclusively
  return wrapper;
}
```

---

## The Full PA Journey — Scene by Scene

### Scene 1: Doctor Visit
Rahul, 28, visits City Medical Center with joint stiffness that has lasted three months.
Dr. Patel diagnoses **Rheumatoid Arthritis (RA)** and prescribes **Humira (adalimumab)** — a biologic that targets the immune protein driving inflammation.
Rahul assumes he can pick it up at the pharmacy. He can't. Humira costs over $6,000/month without insurance — and StarCare Health (an illustrative example payer) requires Prior Authorization before it can be dispensed.

### Scene 2: Insurance Roadblock
Dr. Patel's office submits the PA directly to StarCare Health.
Flow: **Provider → PA Request → Payer.** The pharmacy is not involved at this stage.
Priya explains that once approved, the authorization is saved permanently — Rahul won't repeat this process for Humira within the authorization period.

### Scene 3: What is Prior Authorization?
Priya explains PA in plain language:
- Payers use evidence-based clinical guidelines to define coverage criteria
- **Step therapy** requires cheaper standard treatments to be tried first before approving expensive biologics
- For aggressive diagnoses like RA, step therapy delays can affect disease progression — it is not just bureaucracy

> 📊 **AMA 2023 Prior Authorization Survey:** PA causes treatment delays in the majority of cases, with 94% of physicians reporting PA-related care delays.

### Scene 4: Inside the Insurance Review
StarCare Health checks four things during clinical review:

| Check | Why It Matters |
|---|---|
| **Eligibility** | Coverage gaps or plan exclusions cause instant denials — even if the treatment is appropriate |
| **Clinical Documentation** | Notes, labs, and diagnosis history must match what the payer's criteria require |
| **ICD-10 Diagnosis Match** | Rheumatoid Arthritis = M05.09. The medication must be approved for that exact code — a mismatch denies even clinically correct cases |
| **Step Therapy History** | Documentation must show what was tried, at what dose, for how long, and why it failed |

Rahul took methotrexate for four months before stopping due to elevated liver enzymes and severe GI intolerance. Dr. Patel documented this — but the question is whether it was submitted in the format StarCare Health requires.

### Scene 5: The Denial
**Determination: DENIED**
Reason: Insufficient step therapy documentation. Records submitted did not include dosing dates, duration, or reason for discontinuation.

- A denial is **not permanent** — it is a documentation gap, not a disqualification
- Denial ≠ ineligibility

> 📊 **Industry context:** PA denials cost physician offices an average of **2+ staff hours to resolve** per case (AMA). That time is spent re-gathering records, reformatting documentation, and navigating payer portals.

### Scene 6: Filing the Appeal
The appeal package included:

1. **Methotrexate records** — exact start date, dose (15mg weekly), discontinuation date, clinical reason (AST 3x normal, severe GI intolerance)
2. **Lab results** — objective evidence of enzyme elevation
3. **Letter of Medical Necessity** — Dr. Patel's written argument to StarCare Health's medical director connecting diagnosis, step therapy failure, and clinical rationale for Humira
4. **Personal statement** — Rahul's account of how RA has affected his daily life

Review type requested: **Expedited** (urgent, given documented disease progression).
Expected decision window: 72 hours.

### Scene 7: Approval
**Determination: APPROVED**
Authorization #: AUTH-SH-2024-19203
Valid: 12 months — no repeat PA required for Humira within this period.

Dr. Patel also requested a **Peer-to-Peer (P2P) review** — a direct call with StarCare Health's medical director. P2P reviews overturn denials approximately 40–60% of the time when well-prepared. Most patients don't know this option exists.

### Scene 8: Takeaways

**Patient Perspective — What Rahul Learned:**
- A denial is not a rejection — it's a documentation gap with a fix
- Always ask about the appeal right away — it's a right, not a favor
- Peer-to-Peer review exists and is powerful — ask for it immediately after a denial
- The hardest part is the time. Every day waiting is a day the disease progresses

**System Perspective — How Health Systems Track PA:**

| Metric | What It Measures |
|---|---|
| **Denial Rate** | % of PA requests denied on first submission |
| **Appeal Rate** | % of denials that are formally appealed |
| **Resolution Time** | Days from submission to final decision |

High appeal rates signal that payer criteria may be too restrictive, or that provider submissions have systematic documentation gaps — both of which feed into clinic workflow updates and, sometimes, regulatory review.

> Rahul started Humira 12 days after his appointment.
> The US average PA resolution time is 6–11 days for standard cases.
> For 1 in 4 patients, it exceeds two weeks.

---

## Key Takeaways from Building This

### 1. Writing Dialogue Forces Deeper Understanding Than Reading Does
To put words in Rahul's mouth — "my joints are getting worse and I'm still waiting" — you have to actually understand what he's waiting for and why the delay exists. The act of writing the story compressed the learning.

### 2. The Human Cost Lives Between the Process Steps
Every PA workflow diagram shows boxes and arrows. None of them show the patient on day 8 of waiting, joints worsening, not knowing if the denial is final. Storytelling surfaces what flowcharts can't.

### 3. Same Topic, Two Completely Different Formats — Both Necessary
Day 26 (workflow simulator) showed how the system works. Day 27 (story simulator) showed what it feels like. You need both to fully understand PA. Systems thinking and empathy are not opposites — they're complements.

### 4. `createElement` Discipline Matters
The spec required `createElement + appendChild` for every bubble — never `innerHTML =` on the chat container. Enforcing this prevented an entire class of XSS-style bugs and made the rendering logic explicit and auditable. It's a better pattern than string injection even for personal projects.

### 5. Typing Indicators Are a UX Superpower for Educational Tools
Adding a typing indicator — even a simple CSS-animated 3-dot loader — made the conversation feel like it was happening in real time. That sense of presence increased engagement with the educational content.

### 6. Branching Choices Don't Need Complex State Trees
Both choices in each scene lead to the same next scene — but the *label* of the choice shapes what Priya or Rahul addresses next. The illusion of branching is enough to create engagement. Educational tools don't need full decision trees to feel interactive.

---

## Technical Notes

### Key Implementation Decisions
| Decision | Reason |
|---|---|
| `var` throughout instead of `const/let` | Avoids temporal dead zone issues in older browser environments and eliminates a class of parse errors seen in Day 26 |
| `createElement` only on chat container | Spec requirement; also prevents innerHTML injection bugs |
| Beat delay = `text.length * 18ms` capped at 1200ms | Feels like natural typing speed without being slow for long messages |
| Choices staggered at 80ms intervals | Prevents jarring simultaneous appearance of all buttons |
| Tailwind CDN | Single-file demo — CDN warning is expected and harmless |

---

## Links

- 🔗 GitHub: [github.com/uroojey](https://github.com/uroojey)
- 💼 LinkedIn: [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey)
- 🌐 Portfolio: [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app)

---

*Day 26 build: Prior Authorization Workflow Simulator (drag-and-drop, gamified) → see day26.md*
