# Scope

Frame a research question into a structured scope document that guides all downstream work.

## Trigger phrases
"scope this", "scope this research", "I want to research X", "help me frame this question",
"start a research project on X", "define my research scope", "what should I be asking about X"

## Standalone use
Works from scratch — just tell Claude a topic or question. No prior files needed.

## Purpose

Raw research questions are underspecified. "Tell me about X" leads to sprawl. This skill
forces three clarifications before any searching begins:

1. What is the specific, answerable question?
2. What constraints apply (time range, geography, domain, depth)?
3. What does a useful first-pass answer look like?

The output `scope.md` is a shared reference all other skills can read — but none require it.

## Modes

### Mode A — Assisted (default)
Claude interviews the user with structured questions, then drafts the scope.
Best when the research question is vague or multi-part.

### Mode B — Direct
User provides a fully-formed question and constraints. Claude validates, identifies ambiguities,
and writes the scope file immediately.

### Mode C — Refine
User has an existing `scope.md` and wants to narrow, broaden, or redirect. Claude diffs the
old scope against the proposed change before writing.

## Workflow

**Step 0 — Check for existing scope**
Look for `output/{topic-slug}/scope.md`. If it exists, default to Mode C.

**Step 1 — Gather inputs (Mode A only)**
Ask the user:
- What is the core question you want answered?
- Who is this for and what decision or understanding does it serve?
- What time range matters? (last 3 years / last decade / historical / no constraint)
- Any geographic or domain constraints?
- Any sources, authors, or institutions to prioritize?
- Any perspectives to explicitly exclude?

**Step 2 — Derive sub-questions**
From the core question, generate 3–6 sub-questions that together fully answer it.
Each sub-question should be independently answerable and non-overlapping.

**Step 3 — Propose topic slug**
Derive from the core question: lowercase, hyphen-separated, max 5 words.
Example: "What caused the 2008 financial crisis?" → `2008-financial-crisis`
Confirm with user before saving.

**Step 4 — Write and confirm scope.md**
Save to `output/{topic-slug}/scope.md`. Present it and ask: "Does this capture what you need?"
Apply any corrections before finalizing.

## Output format — scope.md

```
---
topic: {topic-slug}
created: {YYYY-MM-DD}
---

# Research Scope: {Title}

## Core Question
{The single most important question this research answers}

## Audience and Purpose
{Who will read this, and what decision or understanding it serves}

## Sub-Questions
1. {sub-question}
2. {sub-question}
3. {sub-question}
...

## Constraints
- Time range: {e.g., 2018–present / no constraint}
- Geography: {e.g., Global / US-focused}
- Domain: {e.g., peer-reviewed only / includes grey literature / practitioner sources}

## Known Starting Points
{Any URLs, authors, institutions, or search terms the user already knows — or "None"}

## Exclusions
{What to ignore even if relevant-looking — or "None"}
```

## Output
- `output/{topic-slug}/scope.md`

## Hard rule
Never start web searches during scoping. Scope first, search second.
Other skills that read this file: source-discovery, source-evaluation, deep-read, synthesis,
gap-analysis, expert-map, interview-questions, comps — but none of them require it.
