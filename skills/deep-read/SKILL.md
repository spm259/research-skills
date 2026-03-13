# Deep Read

Fetch and closely read a source (or a set of sources), extracting structured notes keyed
to your research questions.

## Trigger phrases
"read this for me", "read this paper", "extract notes from [URL]", "deep read [URL]",
"read and take notes on X", "what does this source say about X", "summarize [URL]"

## Standalone use
The most common standalone usage: "Read this for me: [URL]". No scope.md or prior files needed.
Claude reads the source and returns structured notes immediately.

## Purpose

Deep reading produces one structured note file per source — the raw material that synthesis
and gap-analysis operate on. But each note is also immediately useful on its own as a
reading summary you can reference later.

## Modes

### Mode A — Single source (most common)
User provides one URL or arXiv ID. Claude fetches and reads it fully.

### Mode B — Priority queue
Read sources from `evaluated.md` in descending score order. Claude tracks coverage across
sub-questions and reports when diminishing returns set in.

### Mode C — Sub-question focused
User specifies one sub-question. Claude reads only sources tagged to that sub-question.

### Mode D — Re-read
Re-fetch a source that was read previously (e.g., after scope changed). Overwrites the
existing note file.

## Workflow

**Step 0 — Determine input**
If user provided a URL or arXiv ID: go directly to Step 2 (Mode A).
Otherwise read `output/{topic-slug}/sources/evaluated.md` to get the source queue.
Read `scope.md` if available to orient the notes to the research sub-questions.

**Step 1 — Confirm reading queue (Mode B only)**
Print the list of sources to read. Ask: "Should I read all {n} approved sources, or
just the top {recommended_n}?" Adjust based on user response.

**Step 2 — Fetch source**
Use WebFetch on the source URL.
- For arXiv: prefer `https://ar5iv.labs.arxiv.org/html/{arxiv_id}` over the raw PDF
- If WebFetch returns a redirect: follow once, note the final URL
- If paywall detected: try `https://api.unpaywall.org/v2/{doi}?email={UNPAYWALL_EMAIL}`
- If still blocked: mark `read_status: fetch_failed`, note the reason, move on

**Step 3 — Extract structured notes**

Use this template for every source:

```
## Summary
{2–4 sentences: what is this source's main argument or finding in plain language}

## Key Claims
{Bulleted list of 3–7 specific, factual claims. Include numbers, dates, and proper nouns.}

## Evidence and Methodology
{How does the source support its claims? Primary data, survey, case study, expert opinion,
theoretical argument? Note sample sizes or geographic scope if mentioned.}

## Relevance to Research Questions
{For each sub-question from scope.md (or stated research questions), note:
"Q1: [directly addresses / tangential / not addressed] — {what it says}"}
{If no scope.md exists, note which of the user's stated questions this addresses.}

## Notable Quotes
{1–3 direct quotes worth potentially citing later. Include section or page reference if available.}

## Limitations and Caveats
{What does the source not cover? What methodological weaknesses are apparent? Any potential bias
from funding source, perspective, or publication venue?}

## New Sources to Consider
{Other sources this source cites that aren't in your list yet. Flag high-value ones with "★"}
```

**Step 4 — Save note**
Save to `output/{topic-slug}/notes/{source-slug}.md`
Source slug: lowercase title, hyphen-separated, max 6 words.

**Step 5 — Coverage update (Mode B only)**
After each note, update coverage tracking across sub-questions:
- `low`: 0–1 sources address it
- `partial`: 2 sources address it
- `good`: 3+ sources with varied perspectives
- `saturated`: 5+ sources with high agreement

After every 5 sources, print the coverage matrix and ask whether to continue.

**Step 6 — Flag new sources**
After reading, report any "★" new sources found. Ask: "Add these to the source list?"

## Output format — notes/{source-slug}.md

```markdown
---
source_title: {full title}
url: {url}
authors: {authors}
date: {date}
read_date: {YYYY-MM-DD}
read_status: completed | fetch_failed | partial
---

# Notes: {Title}

## Summary
...

## Key Claims
- ...

## Evidence and Methodology
...

## Relevance to Research Questions
- Q1: ...
- Q2: ...

## Notable Quotes
> "..." — {section/page}

## Limitations and Caveats
...

## New Sources to Consider
- ★ {title} — {why it's worth reading}
```

## Output
- `output/{topic-slug}/notes/{source-slug}.md` (one file per source)

## Hard rule
Never hallucinate source content. If a fetch fails, the note must say so explicitly.
A failed fetch is better than a fabricated note.
