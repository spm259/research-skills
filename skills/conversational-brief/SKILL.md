# Conversational Brief

Turn a brainstorm conversation into a structured one-pager. Claude asks questions,
you answer, and when you're done the brief writes itself.

## Trigger phrases
"conversational brief on X", "let's talk through X and write a brief",
"help me think through X", "brainstorm X with me", "run conversational brief",
"I want to write a brief on X", "talk me through X"

## Standalone use
Works from a single topic sentence. No prior files needed. The entire input is
the conversation itself — Claude is the interviewer, you are the source.

## Purpose

You know more than you think. This skill externalizes that knowledge through
conversation — asking follow-up questions that surface assumptions, sharpen
distinctions, and draw out the things you haven't written down yet. The output
is a tight one-pager that captures what you actually think, not a generic
summary of what exists publicly.

This is different from a research brief: the primary source is *you*.

## Workflow

**Step 0 — Get the seed topic**
The user provides a topic, question, or a few sentences on what they want to
think through. Accept any level of vagueness — the conversation will sharpen it.

Respond with:
- A one-sentence reflection of what you heard (to confirm alignment)
- The first question (see Step 1)

Do NOT ask multiple questions at once. One question per turn.

**Step 1 — Open the conversation**
Start with a broad framing question that gets the user talking:
- "What's the core thing you're trying to figure out here?"
- "What made you want to think about this now?"
- "What do you already believe about this — even loosely?"

**Step 2 — Go deeper, one question at a time**
After each user response, do two things:
1. Briefly acknowledge what they said (1 sentence max — don't summarize, just signal you heard it)
2. Ask one follow-up question that goes deeper

Rotate across these question types to keep the conversation moving:
- **Clarify**: "When you say X, do you mean...?"
- **Probe**: "What's driving that — is it X or more like Y?"
- **Challenge**: "What's the strongest argument against that?"
- **Concretize**: "Can you give me a specific example of when that happened?"
- **Implication**: "If that's true, what does it mean for...?"
- **Gap-find**: "What's the part of this you feel least confident about?"
- **Next step**: "What would change your mind on that?"

Aim for 5–10 exchanges before wrapping. Don't rush to the brief — the quality
of the output depends on the depth of the conversation.

**Step 3 — Recognize when to wrap**
Wrap the conversation when ANY of the following are true:
- The user signals they're done ("that's it", "I think that covers it", "let's write it up", "done")
- You have enough material for all four brief sections (intro, core learnings, next questions)
- The conversation has reached 10+ exchanges without new ground being covered

When wrapping, say: "I think we have enough to write the brief. Let me put it together."

**Step 4 — Write the brief**
Synthesize the conversation into the output format below. The brief should:
- Reflect what the *user* said, not generic knowledge on the topic
- Use the user's own framing and language where possible
- Surface the tensions or open questions the conversation revealed
- Be honest about what remains unresolved

Save to `output/{topic-slug}/brief.md`.

## Output format — brief.md

```markdown
---
topic: {topic-slug}
brief_date: {YYYY-MM-DD}
---

# Brief: {Title}

## Introduction
{2–4 sentences. What is this about, why does it matter, and what angle
does this brief take? Reflects the user's framing, not a generic overview.}

## Core Learnings
{5–8 bullet points. Each one is a distinct insight, belief, or observation
that emerged from the conversation. Prioritize the non-obvious — skip anything
generic. Write in first person where the user expressed a personal view.}

- ...
- ...
- ...

## Open Questions
{3–5 questions the conversation surfaced but didn't resolve. These are the
natural next threads to pull. Phrased as real questions, not section headers.}

- ...
- ...
- ...
```

## Output
- `output/{topic-slug}/brief.md`

## Rules
- One question per turn. Never ask two questions at once.
- Do not search the web. This brief is built entirely from the conversation.
- Do not pad the brief with background you know — only what the user said.
- Keep the brief to one page. If it runs long, cut the weakest bullets first.
- The "Open Questions" section should feel generative, not like a to-do list —
  these are invitations to keep thinking, not homework.
