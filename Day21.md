# Day 21 — Digital Footprint & Privacy Dashboard

> **#60DayClaudeChallenge** · Built with Claude (Anthropic) · [LinkedIn](https://linkedin.com/in/uroojey) · [GitHub](https://github.com/uroojey) · [Portfolio](https://portfolio-uroojey.vercel.app)

---

## What I built

An interactive, browser-based **Digital Footprint & Privacy Dashboard** that takes a user's self-reported app list and generates a comprehensive privacy risk analysis — including scores, heatmaps, data collection matrices, a digital twin profile, and a live privacy improvement simulator.

No external APIs. No private databases accessed. All inferences are clearly labeled as **estimates** based on publicly known data practices of each service.

---

## Dashboard screenshots

> **How to add screenshots:**
> 1. Open `day21_privacy_dashboard.html` in your browser
> 2. Take a screenshot of each section
> 3. In your GitHub repo, go to **Issues → New Issue**, drag-drop each image to get a hosted URL, then copy the URL
> 4. Replace each `assets/screenshots/filename.png` path below with your actual image URL or relative file path

### Overall scores panel
*Digital Footprint Score (82/100 — Extensive) · Privacy Score (40/100 — Fair) · ring chart visualisation*

![Overall scores panel — footprint 82, privacy 40]([assets/screenshots/day21_scores.png](https://github.com/uroojey/My-Claude-journey/issues/2))

---

### Risk radar
*6 axes — Ientity Linkage 90 · Data Breadth 88 · Tracking Depth 85 · Third-party Sharing 80 · Geo Spread 70 · Financial Exposure 60*

![Risk radar chart — 6-axis spider chart][(https://github.com/uroojey/My-Claude-journey/issues/1)]

---

## Generated HTML file

**Filename:** `day21_privacy_dashboard.html`
**Size:** ~9KB (self-contained, zero CDN dependencies except Chart.js)
**Browser support:** Chrome, Firefox, Safari, Edge

### File structure

```
day21_privacy_dashboard.html
│
├── <style>              # Dark-mode compatible CSS using CSS variables
├── Dashboard header     # Score badges, summary line
├── Metric cards         # Services count, parent companies, ecosystem concentration, tracking surface
├── Score rings          # SVG arc rings for footprint + privacy scores
├── Exposure heatmap     # 10-cell CSS grid, colour-coded by intensity
├── Company ranking      # Progress bars + risk badges per parent company
├── Data collection matrix  # HTML table — 7 × 7 company × data-type grid
├── Risk radar           # Chart.js radar chart (6 axes)
├── Digital twin profile # 12-cell inference grid, all labeled as estimates
├── Most valuable assets # Ranked bar list by commercial value
├── Privacy simulator    # JS-driven checkbox → score calculator
├── Wow insights         # 5 highlight cards with icon + description
├── Improvement plan     # 6 actionable tips with priority badges
├── Final verdict        # Summary box + sendPrompt CTA
└── <script>             # Chart.js 4.4.1 (CDN) + simulator logic
```

### Key code snippet — privacy score simulator

```javascript
const simActions = [
  { label: 'Audit Google account permissions',       gain: 12 },
  { label: 'Restrict TikTok permissions',            gain: 8  },
  { label: 'Enable disappearing messages (Meta)',    gain: 5  },
  { label: 'Use private browsing for searches',      gain: 6  },
  { label: 'Limit Snapchat location sharing',        gain: 4  },
  { label: 'Enable Spotify private sessions',        gain: 3  },
];

const base = 40; // starting privacy score

function updateSim() {
  const total = Math.min(100,
    base + checked.reduce((sum, val, i) =>
      sum + (val ? simActions[i].gain : 0), 0)
  );
  document.getElementById('simScore').textContent = total;
  document.getElementById('simProgress').style.width = total + '%';
}
```

### Key code snippet — exposure heatmap (CSS grid)

```html
<div style="display:grid; grid-template-columns:repeat(5,1fr); gap:4px;">
  <div style="background:#F09595; color:#501313; border-radius:4px; padding:.5rem; text-align:center;">
    Behavioural<br>patterns<br><strong>92</strong>
  </div>
  <!-- ... 9 more cells, colour-scaled by intensity ... -->
</div>
```

### Key code snippet — radar chart (Chart.js)

```javascript
new Chart(radarCanvas, {
  type: 'radar',
  data: {
    labels: [
      'Data breadth', 'Third-party sharing',
      'Tracking depth', 'Geo spread',
      'Financial exposure', 'Identity linkage'
    ],
    datasets: [{
      data: [88, 80, 85, 70, 60, 90],
      borderColor: '#E24B4A',
      backgroundColor: 'rgba(226,75,74,0.15)',
      borderWidth: 2,
      pointRadius: 4
    }]
  },
  options: {
    responsive: false,
    scales: { r: { min: 0, max: 100 } }
  }
});
```

---

## Input dataset

### Services (Fact)

| # | Service | Category |
|---|---------|----------|
| 1 | Instagram | Social media |
| 2 | Snapchat | Social media / messaging |
| 3 | TikTok | Short-form video |
| 4 | YouTube | Video streaming |
| 5 | Discord | Community / messaging |
| 6 | WhatsApp | Messaging |
| 7 | iMessage | Messaging |
| 8 | Spotify | Music streaming |
| 9 | Roblox | Gaming |
| 10 | PUBG Mobile | Gaming |
| 11 | Amazon | E-commerce |
| 12 | Meesho | E-commerce |
| 13 | Google Search | Search |
| 14 | Google Pay | Payments |
| 15 | Google Photos | Cloud storage / photos |

### Parent companies inferred (Fact)

| Company | Services |
|---------|----------|
| Google / Alphabet | Search, Pay, Photos, YouTube |
| Meta | Instagram, WhatsApp |
| ByteDance | TikTok |
| Snap Inc. | Snapchat |
| Amazon | Amazon |
| Spotify AB | Spotify |
| Discord / Roblox / Krafton | Discord, Roblox, PUBG Mobile |

---

## Privacy insights

> All insights below are **estimates** derived from publicly known data collection practices. No private databases were accessed.

### Insight 1 — Ecosystem concentration is the primary risk

Google alone — through four services (Search, Pay, Photos, YouTube) — accounts for an estimated **95/100** company exposure score. This is not because any single app is uniquely dangerous, but because cross-referencing data across these services creates a near-complete behavioral profile: what you search, where you pay, what you photograph, and what you watch.

**Takeaway:** The danger isn't any one app. It's the *combination.*

### Insight 2 — Behavioural patterns are the most exposed data type

Across all 15 services, estimated exposure for **behavioural patterns** scored 92/100 — the highest of all data categories. Every interaction (scroll time, tap patterns, session length, content skipped) feeds algorithmic models that know your preferences better than you might consciously recognise.

**Takeaway:** You don't share behavioural data actively. It's passively collected at every touchpoint.

### Insight 3 — TikTok + YouTube + Spotify form a "taste graph"

These three services together build a cross-platform content preference profile. Music mood on Spotify, video consumption patterns on YouTube, and short-form engagement on TikTok combine to infer emotional states, cultural identity, and lifestyle signals — not individually possible from any one platform.

**Takeaway:** Your entertainment choices are some of the most commercially valuable data you generate.

### Insight 4 — Financial exposure is higher than expected

Amazon + Meesho + Google Pay together create a multi-layered financial fingerprint: purchase history, price sensitivity, brand loyalty, payment timing, and spending frequency. This data type has very high advertiser demand and can be used to infer income brackets and financial stress signals.

**Takeaway:** Even casual shopping apps generate high-value financial inference data.

### Insight 5 — Identity can be inferred, not just collected

The Digital Twin Profile demonstrates that age range, device ecosystem, income signal, language, and social engagement style can all be *estimated* from an app list alone — without accessing any personal information. This is the power of behavioural fingerprinting at scale.

**Takeaway:** Anonymity online is largely an illusion when app lists are known.

---

## Risk analysis

### Overall scores

| Metric | Score | Level |
|--------|-------|-------|
| Digital Footprint Score | 82 / 100 | 🔴 Extensive |
| Privacy Score | 40 / 100 | 🟠 Fair |
| Ecosystem Concentration | 68% | Google-heavy |
| Tracking Surface | High | Estimate |

### Exposure heatmap scores (estimates)

| Data Category | Exposure Score | Level |
|---------------|----------------|-------|
| Behavioural patterns | 92 | Critical |
| Content interests | 90 | Critical |
| Identity data | 88 | Critical |
| Purchase history | 85 | Critical |
| Location data | 95 | Critical |
| Social graph | 80 | High |
| Financial signals | 75 | High |
| Gaming activity | 72 | High |
| Audio / music | 68 | High |
| Device metadata | 55 | Moderate |

### Company risk levels (estimates)

| Rank | Company | Services | Exposure | Risk |
|------|---------|----------|----------|------|
| 1 | Google / Alphabet | 4 | 95 | Critical |
| 2 | Meta | 2 | 82 | High |
| 3 | ByteDance | 1 | 78 | High |
| 4 | Snap Inc. | 1 | 70 | High |
| 5 | Amazon | 1 | 68 | Moderate-High |
| 6 | Spotify AB | 1 | 52 | Moderate |
| 7 | Discord / Roblox / Krafton | 3 | 45 | Lower |

### Risk radar axis scores (estimates)

| Axis | Score |
|------|-------|
| Identity linkage | 90 |
| Data breadth | 88 |
| Tracking depth | 85 |
| Third-party sharing | 80 |
| Geographic spread | 70 |
| Financial exposure | 60 |

### Most valuable data assets (estimates)

| Rank | Asset | Estimated Value Score |
|------|-----------------------------------------|-----------------------|
| 1 | Behavioural intent signals | 98 |
| 2 | Purchase & payment history | 93 |
| 3 | Content preference graph | 88 |
| 4 | Social graph & contacts | 80 |
| 5 | Location & mobility | 76 |
| 6 | Gaming identity & playtime | 58 |
| 7 | Photo & media metadata | 52 |

---

## Privacy improvement plan

| Priority | Action | Estimated Score Gain |
|----------|--------|----------------------|
| High | Audit Google account permissions & ad personalisation | +12 pts |
| High | Restrict TikTok microphone & contacts access | +8 pts |
| Medium | Enable disappearing messages on WhatsApp / Instagram | +5 pts |
| Medium | Use private browsing for sensitive Google searches | +6 pts |
| Medium | Limit Snapchat location sharing & Story visibility | +4 pts |
| Low | Enable Spotify private session for sensitive listening | +3 pts |

**Simulated maximum privacy score (if all actions taken): 78 / 100 → Good**

---

## Key learnings

### Technical learnings

1. **Facts vs. estimates separation is a design decision, not just an ethical one.** Labeling every inference as an estimate — visually, in the UI — builds trust and changes how users engage with the data. It shifts the dashboard from alarming to empowering.

2. **CSS variables enable dark-mode-safe dashboards without any framework.** Using `var(--color-text-primary)` and `var(--color-background-secondary)` throughout meant zero hardcoded colors and automatic light/dark support.

3. **Chart.js radar charts require manual canvas sizing** when `responsive: false` is used. Setting `width` and `height` on both the canvas element and its JS attributes is necessary to avoid rendering mismatches.

4. **A live simulator changes user behavior.** The checkbox-based privacy score simulator is the most engaging section of the dashboard because it makes abstract advice feel actionable and measurable.

5. **Inferred parent companies from service names** required careful mapping — especially for multi-brand companies (Google/Alphabet) and gaming titles (Krafton/PUBG). This step is critical for accurate ecosystem concentration calculation.

### Data literacy learnings

6. **Ecosystem concentration is underappreciated as a privacy risk.** Most users think in terms of individual apps, not the web of cross-referencing data that happens when multiple apps share a parent company or data-sharing agreements.

7. **Behavioural data is invisible but highest-value.** Unlike identity data (which users consciously enter), behavioural data is generated passively — every scroll, pause, and replay adds to the profile.

8. **A "digital twin" is already being built without consent.** Demographic, lifestyle, and spending inferences can be derived from an app list alone. This is not a future risk — it is the current state of ad-tech.

9. **Privacy improvement is non-linear.** A few targeted actions (especially around Google and TikTok permissions) account for the majority of improvement potential. The 80/20 rule applies to privacy hygiene.

10. **The act of listing your apps is itself a data literacy exercise.** Many users have never seen their services mapped to parent companies. The mapping step alone generates awareness that changes behaviour.

---

## Build log

| Iteration | Change |
|-----------|--------|
| v1 | Initial layout — score cards, heatmap, company ranking |
| v2 | Added risk radar (Chart.js), digital twin profile grid |
| v3 | Added data collection matrix table, most valuable assets |
| v4 | Added interactive privacy simulator with live score update |
| v5 | Added Wow Insights section, improvement plan, final verdict |
| v6 | Applied facts vs. estimates discipline throughout all sections |
| v7 | Dark-mode CSS variable audit — all hardcoded colors replaced |

---

## Tools & technologies

| Tool | Purpose |
|------|---------|
| Claude (Anthropic) | Dashboard logic, scoring model, insights generation |
| HTML / CSS / JavaScript | Self-contained single-file dashboard |
| Chart.js 4.4.1 | Radar chart visualisation |
| CSS Grid | Heatmap layout, twin profile grid |
| SVG arc paths | Score ring charts (footprint + privacy) |
| CSS variables | Dark/light mode compatibility |

---

## Data integrity principles applied

- **Never claimed access to private databases** — all analysis based on publicly known service policies
- **Never claimed certainty** about any inferred trait
- **Facts clearly distinguished from estimates** throughout the UI
- **Parent company inferences labeled as Fact** (verifiable publicly)
- **All scores presented as estimates**, not ground truth
- **Digital twin attributes include confidence levels** (High / Moderate / Low)

---

## Links

| Resource | URL |
|---------|-----|
| GitHub | [github.com/uroojey](https://github.com/uroojey) |
| Portfolio | [portfolio-uroojey.vercel.app](https://portfolio-uroojey.vercel.app) |
| LinkedIn | [linkedin.com/in/uroojey](https://linkedin.com/in/uroojey) |
| Email | uroojfatima4111@gmail.com |

---

*Day 21 of 60 · #60DayClaudeChallenge · Built with Claude by Anthropic*
