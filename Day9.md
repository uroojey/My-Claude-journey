# Day 9/60: Building NutriScope — Personal Nutrition Intelligence Dashboard

## 🎯 Challenge Overview
As part of Day 9 of the **#60DaysClaudeChallenge**, I engineered **NutriScope**, an advanced, self-contained single-page application (SPA) focused on interactive nutrition tracking and dietary analysis. The core objective was to architect a high-fidelity client-side interface that calculates highly accurate personal metabolic requirements, handles multi-unit nutritional conversions, renders real-time visual progress analytics, and acts as an intelligent rule-based expert system for custom dietary recommendations.

---

## 🛠️ Tech Stack & Architecture
* **Frontend Structure:** Clean, modern Semantic HTML5 elements tailored into a compact, responsive application shell.
* **Styling Engine:** Pure modern CSS3 using strict CSS Custom Properties (variables) for contextual dark-mode themes, adaptive layout layers (Grid & Flexbox mappings for multi-page toggle feel), custom fluid scrollbars, and interactive micro-transitions.
* **Data Visualization:** Integrated `Chart.js` (UMD bundle) rendering lightweight, dynamic, state-reactive bar charts and progress rings.
* **State Management & Core Logic:** Vanilla ECMAScript 6+ functional closures managing localized application state (`profile`, `targets`, `foodLog`) backed by robust synchronized persistent state caching via browser `localStorage`.

---

## 🚀 Key Features Built

### 1. Dynamic Metabolic Target Calculator
* Programmed client-side biometric equations utilizing the refined **Mifflin-St Jeor Equation** to derive Basal Metabolic Rate (BMR):
  * **Male:** $\text{BMR} = 10 \times \text{weight (kg)} + 6.25 \times \text{height (cm)} - 5 \times \text{age (y)} + 5$
  * **Female:** $\text{BMR} = 10 \times \text{weight (kg)} + 6.25 \times \text{height (cm)} - 5 \times \text{age (y)} - 161$
* Derived Total Daily Energy Expenditure (TDEE) by mapping precise physical exertion multipliers (`sedentary: 1.2`, `light: 1.375`, `moderate: 1.55`, `active: 1.725`, `extra: 1.9`).
* Implemented automatic macronutrient allocation splits based on targeted functional health ratios:
  * **Carbohydrates:** 55% of total caloric intake ($4\text{ kcal/g}$)
  * **Protein:** 20% of total caloric intake ($4\text{ kcal/g}$)
  * **Fat:** 25% of total caloric intake ($9\text{ kcal/g}$)
* Incorporated Indian Council of Medical Research (**ICMR / ICNF**) guidelines for precise daily micronutrient profiling (Iron, Calcium, Vitamin C, Vitamin D, and Vitamin B12) adjusted natively by user profile age and biological gender criteria.

### 2. Unit-Adjusted Multi-Metric Food Logging
* Built a comprehensive static database of 20 regional and staple Indian foods containing accurate raw structural macro/micro breakdowns mapped per 100g metrics.
* Formulated unit-to-gram standardizers (`g`, `ml`, `piece`, `cup`, `tbsp`) to enable variable item logging with multi-layered dimensional arithmetic calculations.
* Created interactive inline editing nodes within the data rows allowing real-time quantity overrides that feed back instantly into the main state metrics layer.

### 3. Reactive Insight Panels & Visual Analytics
* Engineered synchronized logic connecting the active log metrics to dynamic dashboards including an automated structural **Energy Balance** ring and a comparative global **Macro Breakdown** chart matrix.
* Configured real-time, priority-sorted array filtering engines to segment consumed nutrients into custom contextual insight grids:
  * **Top Deficiencies:** Tracks and visually outputs any macro/micro category operating at $<80\%$ of daily target.
  * **Top Excesses:** Explicitly flags over-consumption vectors shooting beyond $>120\%$ of target baselines.

### 4. Smart Rules-Engine Recommendations
* Formulated a specialized domain-logic rules engine executing dynamic dietary audit paths based on the saved user profile preference matrix (`vegetarian`, `eggetarian`, `nonveg`).
* Generates actionable dietary advice, smart swaps, and supplementation notes (e.g., flagging critical Vitamin B12 and Iron pathways in vegetarian logging maps, recommending Vitamin C synergy couplings to optimize non-heme iron absorption, and proposing portion reductions during caloric ceilings).

---

## 🧠 Key Learnings & Engineering Takeaways

### 📐 Pure Client-Side State Architectures
Mastered unified single-source-of-truth state mutation flow patterns. When data inputs are altered inside the log table or profile forms, the core application state processes downstream computations immediately across all active view frames (Sidebar pills, Dashboard metrics, Analysis progress bars, Recommendation builders) without generating layout flickers or requiring redundant page reloads.

### 📊 Declarative Data Visualization Refinement
Acquired hands-on expertise manipulating `Chart.js` lifecycle hooks inside vanilla frameworks. Specifically implemented optimization mechanisms (`chart.update('none')`) to execute frictionless real-time data updates across UI controls, ensuring charts dynamically expand and adapt values silently without initiating full canvas canvas destruction loops.

### 🎨 Production-Grade CSS Structural Design
Engineered a deep visual structure using strict theme properties built on cool, desaturated slate-greys (`#111318`, `#16191F`) and vibrant functional neon callout states (`--accent`, `--amber`, `--rose`, `--sky`, `--violet`). Avoided UI-heavy layouts to produce clean, high-performance typography emphasizing clean data hierarchy, layout boundaries, and responsive media breakpoint mappings down to mobile width restrictions (`600px`).

### 🧪 Client-Side Edge Case Prevention & Validations
Structured custom validation strategies blocking empty data nodes or negative numbers inside the numeric tracking elements. Handled initial empty states cleanly using semantic notification modules (`no-profile-banner`, fallback table initializers) to direct user onboarding cleanly prior to mathematical code executions.

---

## 🔮 Next Iteration Goals
1. **Dynamic Custom Food Creator:** Building a module to allow custom ingredient logging beyond the static 20-item base array.
2. **Deep History Mappings:** Transitioning from single-day localized caching into comprehensive multiday IndexedDB sequential objects to generate historical timeline graphs.
3. **Advanced AI Meal Parser:** Leveraging LLM processing nodes via edge APIs to turn simple strings (e.g., *"had 2 rotis and a bowl of yellow dal"*) into direct normalized food metrics.

---
## 🔗 Project Link
* View the source code directly here: [nutri-scope.html](./nutri-scope.html)

**#60DaysOfCode #ClaudeChallenge #WebDevelopment #JavaScript #UIUX #BuildInPublic**
