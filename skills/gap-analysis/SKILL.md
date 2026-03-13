# Gap Analysis

Identify what you still don't know: unanswered questions, missing perspectives,
weak evidence, and the most valuable next steps to deepen your research.

## Trigger phrases
"what am I missing on X", "where are the gaps", "what's still unanswered",
"run gap analysis", "what should I research next", "what perspectives am I missing",
"what's weak in my research"

## Standalone use
Works without any prior files. User can describe what they know about a topic and Claude
identifies what's missing. The more context provided, the more targeted the gaps.

## Purpose

Gap analysis is the **transition from first pass to deep work**. It produces a prioritized
action list: what to find next, who to talk to, which claims need more evidence, and which
perspectives are absent. The output is designed to guide your next research session.

## Modes

### Mode A — From synthesis + scope (default)
Read `synthesis.md` and `scope.md`. Identify gaps systematically against each sub-question.

### Mode B — From notes only
No synthesis yet. Claude reads note files directly and identifies coverage gaps.

### Mode C — From inline description
User describes what they know. Claude identifies what's missing from that description.

### Mode D — Targeted
User asks about gaps in a specific sub-question or claim area only.

## Workflow

**Step 0 — Load context**
Read `output/{topic-slug}/synthesis.md` if available.
Read `output/{topic-slug}/scope.md` if available.
Read `output/{topic-slug}/notes/*.md` if available.
If nothing exists: ask the user to describe what they've learned and what they're researching.

**Step 1 — Coverage assessment per sub-question**
For each sub-question, assess:
- `answered`: at least 3 credible sources with consistent findings
- `partially answered`: some sources, but limited depth or single perspective
- `unanswered`: no sources address it meaningfully
- `contested`: sources disagree with no clear resolution

**Step 2 — Missing perspectives**
What types of sources or viewpoints are absent from the research?
Check for:
- Geographic: Is this US/Western-centric? What does non-Western evidence show?
- Temporal: Is there historical context missing? Is recent data absent?
- Methodological: Is it all qualitative? All quantitative? Missing case studies?
- Stakeholder: Missing practitioner voice? Missing critical/skeptical voice? Missing affected communities?
- Ideological: Is the research only from one school of thought?

**Step 3 — Weak evidence**
Which claims in the synthesis are supported by only one source, or only by low-scoring sources?
Flag these as "needs corroboration."

**Step 4 — Prioritized action list**
Rank the gaps by importance: what's most likely to change your understanding if filled?
For each high-priority gap, suggest a concrete next step:
- A targeted search query
- A type of source to find (e.g., "look for practitioner case studies")
- A person category to talk to (links to expert-map skill)
- A specific claim to verify

**Step 5 — Write gaps.md**
Save to `output/{topic-slug}/gaps.md`.

## Output format — gaps.md

```markdown
---
topic: {topic-slug}
gap_analysis_date: {YYYY-MM-DD}
---

# Gap Analysis: {Topic Title}

## Coverage by Research Question

| Question | Status | Note |
|---|---|---|
| Q1: {sub-question} | answered / partial / unanswered / contested | {brief note} |
| Q2: ... | ... | ... |

## Missing Perspectives

- **Geographic**: {what's missing and why it matters}
- **Temporal**: {what's missing}
- **Methodological**: {what's missing}
- **Stakeholder**: {whose voice is absent}
- **Ideological**: {what school of thought is missing}

## Claims Needing Corroboration

- "{Claim from synthesis}" — currently supported only by {source}. Needs a second source.
- ...

## Priority Action List

### High Priority
1. {Gap} — **Next step**: {specific action}
2. ...

### Medium Priority
...

### Lower Priority
...

## Suggested Skills to Run Next
- `source-discovery` (targeted mode) for: {specific query suggestion}
- `expert-map` for: {which perspective category is most needed}
- `deep-read` for: {specific source or source type to read}
```

## Output
- `output/{topic-slug}/gaps.md`
