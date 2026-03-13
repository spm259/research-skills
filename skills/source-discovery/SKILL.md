# Source Discovery

Find relevant sources across the web, academic databases, and news for a research topic.

## Trigger phrases
"find sources on X", "discover sources", "search for papers on X", "find articles about X",
"what's out there on X", "run source discovery", "find me material on X"

## Standalone use
Works without a scope.md. If none exists, Claude asks: "What topic are you researching and
what specifically do you want to know?" then proceeds with the inline answer.

## Purpose

Transform a research question (from scope.md or directly from the user) into a prioritized
list of sources saved to `output/{topic-slug}/sources/discovered.md`. Uses multiple search
channels to avoid tunnel vision from any single query.

## APIs and tools

| Tool | Covers | Auth |
|------|--------|------|
| WebSearch (built-in) | General web, news, reports | None |
| WebFetch (built-in) | Fetch and parse specific URLs | None |
| arXiv API | Academic preprints (STEM, CS, econ) | None |
| Semantic Scholar API | Cross-discipline papers with citation counts | None (rate-limited) |
| Perplexity sonar-pro | Deep web synthesis with inline citations | PERPLEXITY_API_KEY |

Perplexity significantly improves coverage but is optional. Without it, the skill falls back
to WebSearch + arXiv + Semantic Scholar — still very capable.

## Modes

### Mode A — Broad (default)
4–6 searches across multiple channels. Maximize variety.
Best for: new topics, exploratory research.

### Mode B — Targeted
2–3 precision searches on specific authors, institutions, or source types.
Best for: narrow questions or when the user already has partial sources.

### Mode C — Academic-first
Prioritize arXiv and Semantic Scholar. Use WebSearch only for context.
Best for: scientific or quantitative topics.

### Mode D — Refresh
Re-run discovery on an existing `discovered.md` to find sources published since the last run.
Appends new results only.

## Workflow

**Step 0 — Load context**
Read `output/{topic-slug}/scope.md` if it exists. Extract core question, sub-questions,
constraints (time range, geography, domain), known starting points.
If no scope.md, ask the user for the topic and key questions inline.

**Step 1 — Generate search queries**
For each sub-question, generate 2–3 queries using different angles:
- Direct phrasing of the sub-question
- Key entity names + outcome terms
- Contrastive framing ("criticism of X", "alternatives to Y", "failure of X")
- Date-scoped variant if time range is constrained
Total: 8–15 queries across all sub-questions.

**Step 2 — Execute searches**

2a. **WebSearch** — 5–8 results per query: title, URL, snippet, date.

2b. **arXiv** (for technical/academic topics):
  `https://export.arxiv.org/api/query?search_query={encoded_query}&max_results=10&sortBy=relevance`
  Extract: title, authors, abstract, arxiv_id, published date, PDF URL.

2c. **Semantic Scholar** (for cross-disciplinary topics):
  `https://api.semanticscholar.org/graph/v1/paper/search?query={encoded_query}&fields=title,authors,year,abstract,citationCount,externalIds&limit=10`
  Sort results by citationCount descending.

2d. **Perplexity** (if PERPLEXITY_API_KEY is set):
  Model: `sonar-pro`. Set `search_recency_filter` to match scope time range.
  Prompt: "What are the most important sources, studies, and findings on: {sub-question}?"
  Extract all cited URLs from the response.

**Step 3 — Deduplicate and normalize**
Merge all results. Deduplicate by URL and by title similarity (>85% match = same source).
Normalize each entry:
```
- title: {title}
  url: {url}
  type: web | arxiv | semantic-scholar | news | report
  date: {YYYY-MM-DD or YYYY or unknown}
  authors: {comma-separated or empty}
  snippet: {1–2 sentence description}
  sub_questions_addressed: [1, 3]   # indexes matching scope sub-questions
  discovery_method: websearch | arxiv | semantic-scholar | perplexity
  discovery_priority: high | medium | low
```

**Step 4 — Apply scope constraints**
Filter sources violating scope constraints (date range, geography, language).
Mark as `excluded: {reason}` — do not delete them; transparency matters.

**Step 5 — Rank**
Sort by: (a) breadth of sub-question coverage, (b) citation count where available, (c) recency.
Cap at 60 sources before evaluation.

**Step 6 — Save and summarize**
Write `output/{topic-slug}/sources/discovered.md`.
Print summary: total found, breakdown by type, top 5 high-priority sources.

## Output format — discovered.md

```markdown
---
topic: {topic-slug}
discovery_date: {YYYY-MM-DD}
total_sources: {n}
breakdown: {web: n, arxiv: n, semantic-scholar: n, perplexity: n}
---

# Discovered Sources

## High Priority

### 1. {Title}
- url: {url}
- type: {type}
- date: {date}
- authors: {authors}
- snippet: {snippet}
- sub_questions_addressed: [{list}]
- discovery_priority: high

...

## Medium Priority
...

## Low Priority
...

## Excluded Sources
...
```

## Output
- `output/{topic-slug}/sources/discovered.md`
