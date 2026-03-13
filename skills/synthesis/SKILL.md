# Synthesis

Build a landscape map of what is known, contested, and agreed-upon across your sources.

## Trigger phrases
"synthesize what I've read", "what's the landscape on X", "build a synthesis",
"what do sources agree on", "map the thinking on X", "run synthesis",
"what patterns emerge from these sources"

## Standalone use
Works without note files. User can describe what they've read, paste snippets, or summarize
sources verbally. Claude synthesizes from whatever the user provides.

## Purpose

Synthesis is a **first-pass landscape map** — not a final report. It surfaces the shape of
thinking on a topic: where sources converge, where they conflict, and what the weight of
evidence looks like. The goal is to equip you to ask better questions and identify where
to dig deeper, not to close the loop.

Every claim in the synthesis traces back to a source. Claude adds no new information.

## Modes

### Mode A — From note files (default)
Read all `.md` files in `output/{topic-slug}/notes/`. Synthesize across them.

### Mode B — From inline input
User pastes notes, excerpts, or source summaries directly. Claude synthesizes from those.

### Mode C — Incremental update
User has an existing `synthesis.md` and has added new notes. Claude updates only the
sections affected by the new material.

## Workflow

**Step 0 — Load inputs**
Check for `output/{topic-slug}/notes/*.md`. If they exist, read them all (Mode A).
If no note files, ask the user to provide their notes or source summaries (Mode B).
Read `scope.md` if available to orient synthesis to the sub-questions.

**Step 1 — Identify themes per sub-question**
For each sub-question (from scope.md or stated by user), review all notes that address it.
What do sources say? Where do they agree? Where do they diverge?

**Step 2 — Identify cross-cutting themes**
What themes emerge across multiple sub-questions? What is the dominant framework or
mental model that most sources implicitly use?

**Step 3 — Map contested claims**
Which claims do sources disagree on? Name the specific sources and their positions.
Do not flatten disagreements into false consensus.

**Step 4 — Identify consensus findings**
What do the majority of credible sources agree on with strong evidence?
Be specific — "most sources agree that X happened in Y context" not just "there is consensus."

**Step 5 — Write synthesis.md**
Save to `output/{topic-slug}/synthesis.md`.

## Output format — synthesis.md

```markdown
---
topic: {topic-slug}
synthesis_date: {YYYY-MM-DD}
sources_synthesized: {n}
---

# Synthesis: {Topic Title}

## Landscape Overview
{2–4 sentences: what is the overall shape of thinking on this topic?
What is the dominant lens or framework sources use? Any notable blind spots?}

## By Research Question

### Q1: {sub-question}
**What sources agree on:**
- ...

**Where sources diverge:**
- ...

**Weight of evidence:** {strong / mixed / thin / contested}

### Q2: {sub-question}
...

## Cross-Cutting Themes
{Themes that appear across multiple sub-questions, with source references}

## Contested Claims
{Explicit disagreements between sources — name the sources and their positions}
- Claim: {what is disputed}
  - {Source A} argues: ...
  - {Source B} argues: ...

## Strong Consensus
{What credible sources broadly agree on, with evidence quality noted}
- {Finding} — supported by: {source list}

## What This Synthesis Does Not Cover
{Explicit acknowledgment of what was NOT synthesized — missing perspectives, untouched
sub-questions, or source types not yet consulted}
```

## Output
- `output/{topic-slug}/synthesis.md`

## Hard rule
Every factual claim must be traceable to a note file or user-provided source.
Do not add information from Claude's training knowledge — synthesis only, no new research.
If a claim can't be sourced, flag it as "[unsourced — verify]".
