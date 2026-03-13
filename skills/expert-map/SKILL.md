# Expert Map

Identify who you should talk to about a research topic — categorized by perspective,
with guidance on where to find them and how to approach them.

## Trigger phrases
"who should I talk to about X", "who are the experts on X", "build me an expert map",
"who knows the most about X", "who should I interview about X", "run expert map",
"who are the practitioners / investors / researchers in X"

## Standalone use
Works with just a topic name. No prior files needed. This is one of the most valuable
standalone skills — run it any time you're starting a new research area.

## Purpose

Expert maps surface the human knowledge network around a topic. Who are the people
with lived experience, contrarian views, or deep domain knowledge that published sources
don't capture? The output gives you a structured starting point for primary research —
informational interviews, conference conversations, and cold outreach.

## Categories to map

For each topic, consider all six categories. Some may not apply:

| # | Category | Examples |
|---|---|---|
| 1 | **Academic researchers** | Professors, PhD researchers, think tank fellows |
| 2 | **Practitioners / operators** | Founders, executives, product managers, front-line workers |
| 3 | **Investors** | VCs, PE investors, angels, family offices active in this space |
| 4 | **Journalists / analysts** | Beat reporters, industry analysts, newsletter writers |
| 5 | **Policy / regulatory** | Regulators, lobbyists, policy advocates, government staff |
| 6 | **Critics / contrarians** | Skeptics, short sellers, academics who argue against the consensus |

## Modes

### Mode A — Broad (default)
Map all 6 categories with 3–5 entries each.

### Mode B — Targeted
User specifies one or more categories. Only map those.
Example: "I only need investors and practitioners."

### Mode C — Network-aware
User describes their existing network. Claude identifies only the categories/archetypes
NOT yet represented — i.e., the gaps in their current network.

## Workflow

**Step 0 — Load context**
Read `output/{topic-slug}/scope.md` if available.
If no scope.md: ask the user "What topic are you mapping experts for, and what angle matters most?"

**Step 1 — Identify categories relevant to this topic**
Not all 6 categories are equally relevant for every topic. State which categories are most
important for this specific topic and why.

**Step 2 — For each category, identify archetypes and real names**

For each category:
- Define 2–3 *archetypes* (role descriptions that would have this perspective)
- Use WebSearch to find 2–4 real names per category where possible
  - Academic: search "{topic} researcher site:scholar.google.com" or Semantic Scholar
  - Practitioners: search "{topic} founder OR CEO" + company names
  - Investors: search "{topic} investor" + "portfolio" + firm names
  - Journalists: search "{topic} journalist" + publication names
  - Policy: search "{topic} regulator" OR "{topic} policy" + government or NGO
  - Critics: search "{topic} skeptic" OR "criticism of {topic}" OR "{topic} short seller"

Flag all real names as "verify before outreach" — do not treat search results as definitive.

**Step 3 — For each entry, complete the profile**

```
- archetype: {role title or description}
  why_valuable: {what unique perspective or knowledge they have — be specific}
  find_them: {LinkedIn search terms, conference names, publication venues, substack, X/Twitter}
  example_names: {2–4 real people — flag as "verify"}
  suggested_approach: {cold email / conference Q&A / reply to their writing / mutual intro}
  good_opening_question: {one concrete question that would start the conversation well}
```

**Step 4 — Identify the most important single conversation**
Which one person or archetype — if you could only make one call — would most move your
understanding forward? Name them and explain why.

**Step 5 — Write expert-map.md**
Save to `output/{topic-slug}/expert-map.md`.

## Output format — expert-map.md

```markdown
---
topic: {topic-slug}
map_date: {YYYY-MM-DD}
---

# Expert Map: {Topic Title}

## Most Valuable Single Conversation
{Who and why — the one call that would move your understanding most}

## Academic Researchers
### Archetype: {e.g., "Empirical researcher studying X outcomes"}
- why_valuable: ...
- find_them: ...
- example_names: {Name 1} (verify), {Name 2} (verify)
- suggested_approach: ...
- good_opening_question: "..."

...

## Practitioners / Operators
...

## Investors
...

## Journalists / Analysts
...

## Policy / Regulatory
...

## Critics / Contrarians
...

## Coverage Notes
{Any categories that don't apply to this topic, and why}
```

## Output
- `output/{topic-slug}/expert-map.md`

## Notes
- Always flag example names as "verify" — search results can be stale or misattributed.
- The "good_opening_question" per entry directly feeds into the `interview-questions` skill.
- For investment topics: the Investors category often has the most publicly searchable signal
  (firm websites, portfolio pages, Substack posts, conference talks).
