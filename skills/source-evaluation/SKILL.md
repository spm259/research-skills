# Source Evaluation

Score a list of sources on credibility, relevance, recency, and methodology to identify
which ones are worth reading deeply.

## Trigger phrases
"evaluate these sources", "score these sources", "which sources should I read",
"prioritize my sources", "run source evaluation", "which of these is worth reading"

## Standalone use
Works without a scope.md or discovered.md. User can paste a list of URLs or source titles
directly into the conversation. Claude will ask for the research topic if not provided.

## Purpose

Not all sources are equal. This skill evaluates each source on four dimensions and produces
a scored list that tells you what to read deeply vs. skim vs. skip. The output feeds into
`deep-read` but is also useful on its own as a triage tool.

## Scoring dimensions (1–5 each, total 4–20)

| Dimension | What it measures |
|---|---|
| Credibility | Author/publication reputation, peer review status, citation count for academic sources |
| Relevance | How directly the source addresses the research question/sub-questions |
| Recency | How current the information is relative to the scope time range |
| Methodology | Quality of evidence: primary data, rigorous analysis, expert knowledge vs. opinion |

**Thresholds:**
- Score 16–20: `must_read` — prioritize these
- Score 12–15: `approved` — read if time allows
- Score 8–11: `skim` — useful for background or a single data point
- Score 4–7: `skip` — low value; note why

## Modes

### Mode A — Full evaluation (default)
Evaluate all sources from `discovered.md`. WebFetch any ambiguous sources to check content.

### Mode B — Quick triage
Score purely from title, URL domain, and snippet. No fetching. Fast but less accurate.

### Mode C — Spot check
User provides 1–5 specific sources to evaluate in depth.

## Workflow

**Step 0 — Load sources**
Read `output/{topic-slug}/sources/discovered.md` if it exists.
If not, ask the user to paste their source list.
Read `scope.md` if available for relevance scoring context.

**Step 1 — Score each source**

For each source:

*Credibility (1–5):*
- 5: Peer-reviewed journal / established institution / cited 200+ times
- 4: Reputable publication / known expert author / cited 50–199 times
- 3: Credible outlet, unknown author credentials / cited 10–49 times
- 2: Personal blog, advocacy org, unknown outlet / cited <10 times
- 1: No author, no publication date, no institutional affiliation

For academic sources: use citation count from Semantic Scholar data if available.
For non-academic sources: use WebFetch on the domain's About page if credibility is unclear.

*Relevance (1–5):*
- 5: Directly answers a sub-question
- 4: Addresses the core topic with useful data or perspective
- 3: Tangentially relevant — useful for context
- 2: Topic overlap but different angle
- 1: Minimal connection to research question

Score from snippet + title first. Use WebFetch only if the snippet is ambiguous.

*Recency (1–5):*
Score relative to the scope time range. If no time range: use absolute recency.
- 5: Within the last year / most current available
- 4: Within 1–3 years
- 3: Within 3–5 years
- 2: Within 5–10 years
- 1: More than 10 years old (unless topic is historical)

*Methodology (1–5):*
- 5: Primary empirical data, rigorous methodology, peer-reviewed
- 4: Original analysis or expert synthesis with clear evidence base
- 3: Secondary research or expert opinion with cited sources
- 2: Commentary or opinion without clear evidence
- 1: Anecdote, marketing content, or unverified claim

**Step 2 — Compute total and assign status**
Total = credibility + relevance + recency + methodology.
Assign: `must_read` (16+), `approved` (12–15), `skim` (8–11), `skip` (<8).

**Step 3 — Write evaluated.md**
Save to `output/{topic-slug}/sources/evaluated.md`.
Print a summary: counts by status, list of all `must_read` sources.

## Output format — evaluated.md

```markdown
---
topic: {topic-slug}
evaluation_date: {YYYY-MM-DD}
total_evaluated: {n}
must_read: {n}
approved: {n}
skim: {n}
skip: {n}
---

# Evaluated Sources

## Must Read

### 1. {Title}
- url: {url}
- scores: credibility={n} relevance={n} recency={n} methodology={n} total={n}
- evaluation_status: must_read
- rationale: {1–2 sentences explaining the score}

...

## Approved
...

## Skim
...

## Skip
...
```

## Output
- `output/{topic-slug}/sources/evaluated.md`
