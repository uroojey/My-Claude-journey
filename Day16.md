# Day 16 of 60 — Building a Stock Research Skill & Analyzing Infosys

**Challenge:** #60DayClaudeChallenge
**Category:** Custom Skill Creation + Financial Research
**Tool used:** Claude (Sonnet) + custom `stock-fundamental-research` skill
**Date:** June 16, 2026

---

## What I built today

On Day 16, I designed and deployed a custom **stock fundamental research skill** for Claude
— a structured prompt system that turns any stock name into a cited, multi-mode fundamental research report.

The skill supports five modes:

| Mode | When it activates |
|---|---|
| Quick Take | Single stock name or short request (default) |
| Deep Dive | "Full analysis" or "detailed" request |
| Compare | Two stocks or "vs / compare" in the request |
| Pros & Cons | Strengths and weaknesses request |
| Portfolio Fit | User shares holdings + asks how a stock fits |

Key design principles baked in:
- Live data first (Screener.in → Tickertape → Moneycontrol → NSE/BSE → SEC filings)
- Every figure cited with a source — no fabricated data
- Mandatory disclaimer on every output — not investment advice
- Color-coded metric interpretation (green / amber / red thresholds)
- Covers 20+ data points: P/E, P/B, ROE, ROCE, D/E, FCF, EPS trend, promoter holding, FII/DII, moat, peers, and more

---

## Stock analyzed: Infosys (NSE: INFY)

I ran the Quick Take mode on **Infosys Ltd**, India's second-largest IT company.

### Key metrics surfaced (as of June 2026)

| Metric | Value | Source |
|---|---|---|
| CMP | ~₹1,116 | Tickertape |
| Market cap | ₹4.63 lakh crore | Screener.in |
| 52W range | ₹1,089 – ₹1,728 | Tickertape |
| P/E ratio | 16.2× | Tickertape |
| IT sector avg P/E | ~22× | Sector benchmark |
| D/E ratio | 0.10× | financecharts.com |
| ROE (3Y avg) | 30.8% | Screener.in |
| Current ratio | 1.81× | stockanalysis.com |
| Free cash flow | $3.73B (FY26) | Infosys SEC filing |
| EPS growth | +11% YoY (INR) | Infosys FY26 results |
| Dividend yield | ~4.1% | Screener.in |
| Promoter holding | 14.4% | kotakneo.com |
| FY26 revenue growth | 3.1% (constant currency) | Infosys SEC filing |
| FY27 revenue guidance | 1.5–3.5% CC | Infosys earnings call |

### Fundamental quality verdict: **Strong**

The skill assessed Infosys as fundamentally strong on balance sheet metrics — near-zero debt,
90%+ FCF conversion, sustained 30%+ ROE — but flagged slowing growth as the primary drag on valuation.

---

## What surprised me most

**The valuation gap.**

Infosys is trading at a P/E of ~16×. The broader IT sector average is ~22×. That's a **27% discount** for a company with:
- 30.8% average ROE over 3 years
- Debt-to-equity of just 0.10×
- $3.73 billion in free cash flow
- A dividend yield of ~4.1%

I expected a strong balance sheet. I didn't expect the stock to look this discounted relative to its own sector.

The skill surfaced the reason immediately: revenue growth of only 3.1% in FY26, and cautious guidance of 1.5–3.5% for FY27.
The market is pricing in a slow-growth environment — and it's not wrong.

That tension — **fortress balance sheet vs muted growth outlook** — is the kind of nuance that would normally take an hour of manual research.
The skill surfaced it in under a minute, with every data point cited.

---

## Skill architecture: how it works

```
User prompt: "Infosys"
        ↓
Mode detection → Quick Take (default: single stock, no mode keyword)
        ↓
Live data fetch → Screener, Tickertape, SEC filings (cross-checked)
        ↓
Research checklist → 20+ metrics gathered and interpreted
        ↓
Interpretation engine → Green / Amber / Red thresholds applied
        ↓
Output → CMP, valuation verdict, health snapshot, 3 strengths, 2 watch-points,
         Fundamental Quality verdict, price chart, closing disclaimer
```

---

## Skill file structure

```
stock-fundamental-research/
└── SKILL.md          ← full skill definition with modes, rules, checklist,
                         interpretation thresholds, and output format specs
```

---

## Key learnings from Day 16

1. **Skill design is prompt engineering at a higher level.** Writing a skill means thinking about edge cases, output consistency, and failure modes —
not just "what should Claude say."

2. **Mandatory rules matter.** The no-fabrication rule and mandatory disclaimer aren't restrictions — they're what make the output trustworthy and shareable.

3. **AI-assisted research doesn't replace judgment.** It compresses the time to reach *informed questions*.
The valuation gap in Infosys is only useful if you understand *why* it exists.

4. **Source discipline is everything in finance.** The skill's insistence on citing every figure forced me to cross-check data across 4+ sources
— a habit that improves research quality regardless of the tool.

---

## Tools & references

- Claude Sonnet (claude-sonnet-4-6) — primary research engine
- Screener.in — ROE, dividend data
- Tickertape — CMP, P/E, 52W range, market cap
- stockanalysis.com — D/E, current ratio, ROE
- financecharts.com — D/E trend
- kotakneo.com — shareholding pattern
- Infosys FY26 SEC filings (Form 6-K) — revenue, FCF, EPS, guidance

---

*This analysis was generated using a custom Claude skill for educational and learning purposes only. It is not investment advice. All figures should be independently verified.*
