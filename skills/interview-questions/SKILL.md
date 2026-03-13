# Interview Questions

Build tailored informational interview questions for a specific person or role.

## Trigger phrases
"build interview questions for [person/role]", "prep questions for my call with [person]",
"I'm talking to [role] about X, help me prepare", "what should I ask [person]",
"run interview questions", "build me a question guide for [person]"

## Standalone use
Works with just: "I'm talking to [person/role] about [topic]." No prior files needed.
The minimum viable input is a person's role and a rough topic.

## Purpose

Informational interviews are one of the most efficient ways to compress research time —
but only if the questions are well-designed. This skill builds a structured question guide
tailored to both the specific person's vantage point and your current research gaps.
Questions are open-ended, conversational, and hypothesis-testing — designed to surface
what no published source will tell you.

## Modes

### Mode A — Role-based (default)
User provides a role/archetype (e.g., "VP of Product at a construction SaaS company").
Claude tailors questions to that role's typical knowledge and blind spots.

### Mode B — Person-specific
User provides a specific named person with known background.
Claude researches them (WebSearch/WebFetch their writing, talks, LinkedIn) before generating
questions, then tailors questions to their specific perspective.

### Mode C — Category mode
Matched to categories from `expert-map.md`:
- `expert-academic`: research methodology, open questions in the field, what practitioners get wrong
- `expert-practitioner`: lived experience, what textbooks miss, day-to-day reality
- `expert-investor`: market sizing, competitive dynamics, why bets were made or passed
- `expert-critic`: strongest counterarguments, what believers get wrong

### Mode D — Refresh
User has already run this interview and wants follow-up questions based on what they learned.

## Workflow

**Step 0 — Load context**
Read `output/{topic-slug}/scope.md` if available — use sub-questions to frame the interview.
Read `output/{topic-slug}/gaps.md` if available — prioritize questions that address high-priority gaps.
Read `output/{topic-slug}/synthesis.md` if available — use to build hypothesis-testing questions.
If in Mode B: WebSearch the person's name + company, fetch their writing or talks if findable.

**Step 1 — Clarify inputs (if needed)**
If the user hasn't specified:
- Who is this person and what is their role?
- What is the core topic of the interview?
- What do you most want to learn from this specific person?
- Do you have any hypotheses you want tested?
- How long is the interview? (30 min = ~8 questions, 60 min = ~15 questions)

**Step 2 — Draft the context brief**
1 paragraph explaining:
- What this person's vantage point is
- What they likely know well (and what they probably don't)
- What makes this conversation valuable given your current research state

**Step 3 — Build the question guide**

Structure:

**Opening (2–3 questions)** — Rapport + framing. Get them talking comfortably.
- Questions about their path and role, not the topic yet
- Signal that you've done homework: reference something specific they've written or done

**Core questions (8–12 questions)** — Organized by topic area, easy to hard within each area.
- Each question is open-ended (starts with "How", "What", "Tell me about", "Walk me through")
- No yes/no questions
- Each question has a 1-line note: *[What you're trying to learn]*
- Include probes: "Can you give me a specific example?" follow-ups planned

**Hypothesis-testing questions (3–5 questions)** — Present your current understanding and ask them to poke holes.
Format: "My current sense is [X]. Does that match your experience, or do you see it differently?"
These are the highest-value questions — they surface what you're getting wrong.

**Forward-looking questions (2–3 questions)**
- Where is this heading?
- What would change your mind?
- What question are people not asking that they should be?

**Closing (2 questions — always the same)**
- "Who else should I talk to that would give me a very different perspective?"
- "Is there anything you'd recommend I read that I probably haven't found yet?"

**Step 4 — Write question file**
Save to `output/{topic-slug}/interview-questions/{person-slug}.md`
Person slug: role or name, lowercase, hyphen-separated (e.g., `vp-product-construction-saas`)

## Output format — interview-questions/{person-slug}.md

```markdown
---
topic: {topic-slug}
interview_date: {planned date or TBD}
person: {name or role description}
interview_mode: academic | practitioner | investor | critic
---

# Interview Guide: {Person/Role}

## Context Brief
{1 paragraph: their vantage point, what they know well, why this conversation is valuable
given your current research state}

## Opening (rapport + framing)
1. {Question}
   *[What you're trying to learn]*

2. {Question}

## Core Questions

### {Topic Area 1}
3. {Question}
   *[What you're trying to learn]*
   Probe: "{follow-up if they give a short answer}"

4. {Question}

### {Topic Area 2}
...

## Hypothesis-Testing
{n}. My current sense is {hypothesis}. Does that match your experience, or am I
    missing something?
    *[Testing: {which belief you want challenged}]*

## Forward-Looking
{n}. {Question}

## Closing
{n}. Who else should I talk to who would give me a very different perspective?
{n+1}. Is there anything you'd recommend I read that I probably haven't found yet?

## Notes for After the Interview
{Any specific things to listen for, or red flags that would suggest you need to probe deeper}
```

## Output
- `output/{topic-slug}/interview-questions/{person-slug}.md`

## Design rules
- Every question must be open-ended. No yes/no questions.
- No leading questions. Questions should invite challenge, not confirmation.
- Hypothesis-testing questions are the most important — prioritize 3+ of them.
- The closing questions ("who else?" and "what to read?") are non-negotiable — always include them.
- Time the guide: 30-min interview = max 10 questions; 60-min = max 18 questions.
