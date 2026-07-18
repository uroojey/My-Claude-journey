# The Verdict Engine
### Day 48/60 — #60DayClaudeChallenge

A structured decision-support tool built with Claude: **Cloud Hosting Free Tiers — Compare & Decide**, designed for students and learners choosing a provider for a free-tier learning project.

---

## 1. What It Is

The Verdict Engine is a single-file HTML application that compares **AWS, Google Cloud, Microsoft Azure, and DigitalOcean** across five criteria relevant to a student picking a provider to learn on:

1. Free-tier compute limits
2. Free-tier duration
3. Ease of setup for beginners
4. Free storage & database limits
5. Learning resources & certifications

Rather than jumping straight to output, Claude interviewed me first — one question at a time — to pin down:
- The category and real comparable options
- The target user and the one decision they need confidence on
- Measurable criteria (at least four)
- Real, citable data sources per criterion
- Whether to allow weighted priorities or show one fixed ranking

Only after that interview did it research and build.

**Deliverable:** [`cloud-free-tier-compare.html`](attachment) — a self-contained HTML/CSS/JS app (no external libraries) with:
- A fixed, equal-weighted ranking
- An optional weight-sandbox slider panel
- A full sourced comparison table, with estimates clearly flagged (`EST`)
- A live "Sources" panel listing every citation
- A collapsible "How this was researched" panel explaining data provenance and conflicts resolved
- Graceful handling of loading/empty/error states

---

## 2. Sourced Data Report

| Provider | Compute | Duration | Setup | Storage | Learning | Key Sources |
|---|---|---|---|---|---|---|
| **AWS** | New accounts: $100–200 credit, ~6mo window. Legacy accounts: 750 hrs/mo EC2 for 12mo. | Changed Jul 15, 2025 — no longer a flat 12mo for new signups. | Console complexity + billing traps (NAT Gateway ~$33/mo) widely reported. | Legacy: 5GB S3 + 30GB EBS + 750hrs RDS (12mo). New: shared credit pool. Lambda/DynamoDB always free. | Largest ecosystem — AWS Skill Builder, certifications, largest 3rd-party tutorial volume. | agentdeals.dev, oneuptime.com, infratally.com, cloudwebschool.com, costbench.com |
| **Google Cloud** | $300 credit/90 days **+** permanent Always Free e2-micro VM (never expires). | Always Free tier (VM, 5GB storage, 2M Cloud Run reqs, 1TB BigQuery/mo) is perpetual, separate from the 90-day credit. | Also has 100+ products; described as intimidating for first login. | 5GB Cloud Storage + 1TB BigQuery queries/mo, permanent. | Google Cloud Skills Boost + certifications; smaller tutorial ecosystem than AWS. | cloud.google.com (official), docs.cloud.google.com (official), gturanker.org, thebestblogever.co |
| **Azure** | $200 credit/30 days + 12mo free popular services + 55+ always-free services. | $200 credit window is shortest of the three majors (30 days), but 12mo + always-free extend well beyond. | Azure for Students: $100 credit, no card required — lowers entry barrier specifically for students. | 12mo tier: limited Blob Storage + small SQL DB instance; 55+ always-free services with modest caps. | Microsoft Learn — free, structured, tied to AZ-900 and other certifications. | azure.microsoft.com (official), medhacloud.com, acom-sandbox.azure.net (official) |
| **DigitalOcean** | $200 credit/60 days via signup; no long-standing free compute after. | 60-day window — between Azure's 30 and GCP's 90; no credit rollover. | Consistently rated most beginner-friendly dashboard; 100+ one-click apps; flat/predictable pricing. | No dedicated always-free storage/DB; App Platform gives 3 free static sites (HTTPS + CDN) permanently. | Strong community tutorials; no formal certification track. | agentdeals.dev, GitHub Gist review, problogguru.com, costgoat.com |

**Full citation list** (18 sources) is rendered live in the app's Sources panel, each linked to its original URL with publisher and date.

---

## 3. Most Surprising / Useful Data Point

**AWS quietly overhauled its entire free-tier structure on July 15, 2025 — and most guidance hasn't caught up.**

The "750 hours/month of EC2 for 12 months" that most people associate with AWS only applies to accounts created *before* that date. New accounts instead receive a one-time $100–$200 credit pool that typically runs out in around six months — not a renewing annual allowance.

Meanwhile, Google Cloud's Always Free e2-micro VM **never expires**, run independently of its 90-day trial credit.

**The practical implication:** the provider with the strongest reputation for "generous free tier" is, as of 2026, actually the *least durable* free option for a brand-new learner — the opposite of what most tutorials, blog posts, and even some evergreen "getting started" guides still imply. Catching this required cross-checking dated sources against each other rather than trusting any single one, which is exactly where structured, citation-backed AI research earns its keep over a quick web search.

---

## 4. Key Learnings

1. **Interview-first beats output-first.** Forcing the criteria, audience, and sourcing rules to be defined *before* any research happened prevented scope creep and kept the final tool genuinely decision-useful rather than a generic feature comparison.
2. **Freshness matters more than authority.** Several "official-sounding" pages were stale relative to the July 2025 AWS policy change. Cross-referencing publish/update dates across independent sources caught a conflict a single source wouldn't have revealed.
3. **Flagging estimates builds trust.** Not every criterion (e.g., "ease of setup") has a single hard published number. Explicitly marking those as `EST` in the UI — rather than presenting all scores with false precision — makes the tool more credible, not less.
4. **Transparency panels aren't optional extras.** The "How this was researched" panel, documenting exactly which conflicts were found and how they were resolved, turned out to be as valuable as the ranking itself for a user trying to decide how much to trust the verdict.
5. **Equal-weighting was the right default for this audience.** Students don't yet know which criterion matters most to them — a fixed, transparent ranking with an optional weight sandbox served that audience better than forcing an early prioritization decision.

---

*Built with Claude — Day 48 of 60. #60DayClaudeChallenge*
*Data verified as of July 18–19, 2026. Cloud free-tier terms change frequently — always confirm current limits on each provider's official pricing page.*

@Anthropic @ABTalksOnAI @AnilBajpai
