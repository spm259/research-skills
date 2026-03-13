# Comps

Build a comparable companies or precedent transactions table for an investment topic.

## Trigger phrases
"build comps for X", "find comparable companies to X", "what are the comps for X",
"run comps", "build a comps table for X", "find precedent transactions in X",
"what companies are similar to X", "build me a trading comps table"

## Standalone use
Works with just a company name, sector description, or deal type. No prior files needed.
This is a fully independent skill optimized for investment research.

## Purpose

Comps ground your investment analysis in market reality — what are similar businesses
worth, how have similar deals been structured, and what does the competitive landscape
look like? This skill produces a structured first-pass table from publicly available
information (news, filings, press releases, Crunchbase data in media coverage), with
explicit flags on data quality and recency.

**Important**: This is a first-pass research tool. Metrics sourced from news estimates
should be treated as directional, not as verified figures for a financial model.

## Modes

### Mode A — Public company comps (trading comps)
Find publicly listed companies in the same sector/business model.
Pull available public metrics: revenue (range or estimate), growth rate, key valuation
multiples (EV/Revenue, EV/EBITDA, P/E) from earnings releases, analyst coverage, or
financial news. Flag whether figures are disclosed vs. estimated.

### Mode B — Private company comps
Find VC or PE-backed companies at a similar stage, business model, or market.
Pull: last known funding round, valuation (if disclosed), revenue range if mentioned in press.
Sources: Crunchbase coverage in media, PitchBook deal announcements, press releases.

### Mode C — Precedent transactions
Find M&A deals, acquisitions, or investment rounds in the space.
Pull: deal size, implied valuation, buyer/investor, strategic rationale, date.
Sources: press releases, SEC filings (for public deals), financial news.

### Mode D — Business model comps
Find companies with analogous business models in adjacent or different sectors.
Useful for early-stage companies where direct sector comps don't exist.
Focus on: revenue model analogy, unit economics structure, go-to-market parallels.

### Mode E — Combined
Run Modes A + B + C together for a comprehensive comps set.

## Workflow

**Step 0 — Clarify inputs**
Confirm with the user:
- Subject company or sector (what are we building comps for?)
- Mode (or ask: "Do you want public comps, private comps, precedent transactions, or all three?")
- Geography scope (global / US-only / specific region?)
- Stage focus (for private comps: seed / Series A / growth / pre-IPO?)

Read `output/{topic-slug}/scope.md` if available for additional context.

**Step 1 — Define the comparable set criteria**
Before searching, state explicitly:
- What makes a company "comparable" for this analysis?
  (e.g., same business model, same customer segment, same revenue range, same geography)
- What makes a company NOT comparable?
  (e.g., different monetization model, different scale, regulated differently)

These criteria guide the search and help the user evaluate the output.

**Step 2 — Search for comps**

For each mode:

*Public comps*:
- WebSearch: "{sector} public companies", "{business model} listed companies", "{subject company} competitors site:finance.yahoo.com OR site:sec.gov"
- For each candidate: WebFetch their latest earnings release or investor presentation for metrics
- Pull EV/Revenue and EV/EBITDA multiples from financial news or analyst coverage

*Private comps*:
- WebSearch: "{sector} startup funding 2023 OR 2024", "{business model} Series B OR Series C"
- WebFetch Crunchbase profile pages, press release pages for funding announcements
- Note: private valuations are rarely disclosed; use "last known round" as proxy

*Precedent transactions*:
- WebSearch: "{sector} acquisition 2022 OR 2023 OR 2024", "{sector} M&A deal"
- For public transactions: WebFetch SEC 8-K or press release for deal terms
- Pull: acquirer, target, deal size, implied multiple if stated, strategic rationale quote

**Step 3 — Build the table**

Standard columns (adapt by mode):

| Company | HQ | Founded | Stage/Status | Business Model | Key Metric | Valuation / Multiple | Date | Source | Comparability Notes |
|---|---|---|---|---|---|---|---|---|---|

Key metric varies by sector:
- SaaS: ARR or ARR range
- Marketplace: GMV
- Fintech: revenue or AUM
- Consumer: MAU or revenue
- Industrial: revenue

**Step 4 — Write Limitations section**
Always include:
- Which metrics are from disclosed sources vs. news estimates vs. inferred
- Date of most recent data point for each company
- Companies where comparability is imperfect and why
- Data gaps (companies where no metrics were findable)

**Step 5 — Write comps.md**
Save to `output/{topic-slug}/comps.md`.
Print a 2–3 sentence summary: valuation range observed, key outliers, what this implies.

## Output format — comps.md

```markdown
---
topic: {topic-slug}
comp_date: {YYYY-MM-DD}
subject: {company or sector being analyzed}
modes_run: {public | private | transactions | business-model}
---

# Comps: {Subject}

## Comparability Criteria
**Included if:** {criteria}
**Excluded if:** {criteria}

## Public Company Comps

| Company | HQ | Business Model | Key Metric | EV/Rev | EV/EBITDA | Source | Data Date | Notes |
|---|---|---|---|---|---|---|---|---|
| {Company} | {HQ} | {model} | {metric} | {x}x | {x}x | {URL} | {date} | {notes} |
...

## Private Company Comps

| Company | Stage | Last Round | Valuation | Key Metric | Source | Data Date | Notes |
|---|---|---|---|---|---|---|---|---|
| {Company} | {stage} | {Series X, $Xm} | {$Xm or undisclosed} | {metric} | {URL} | {date} | {notes} |
...

## Precedent Transactions

| Target | Acquirer/Investor | Deal Size | Implied Multiple | Date | Rationale | Source |
|---|---|---|---|---|---|---|
| {Target} | {Buyer} | ${X}m | {x}x Rev | {date} | "{quote}" | {URL} |
...

## Summary Observations
{2–4 sentences: valuation range, what the data implies for the subject company,
notable outliers and why}

## Limitations
- {Company X}: revenue figure from news estimate ({source}), not disclosed filing
- {Company Y}: last valuation from {year}, may be stale
- {Company Z}: business model overlap is imperfect because {reason}
- Data gaps: no public metrics found for {list of companies}
```

## Output
- `output/{topic-slug}/comps.md`

## Notes
- Always cite the source URL and data date for every metric — this is a research tool,
  not a financial model. Source quality matters.
- For private companies, "undisclosed" is a valid and honest entry — do not fabricate.
- If no comps are findable for a specific mode, say so explicitly rather than stretching
  the comparability criteria to fill the table.
- Business model comps (Mode D) are often the most insightful for early-stage analysis —
  don't skip them just because they're from a different sector.
