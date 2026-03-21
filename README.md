# Research Skills

A Claude Code skills toolkit for in-depth research on any topic. Go from a raw question
to a structured first pass — with sources, synthesis, expert map, interview guides,
and investment comps — in a single session.

---

## What this is

A set of 10 modular Claude Code skills that handle each stage of a research process.
Use them as a pipeline or invoke any skill standalone — they all work independently.

The goal is a **first pass that empowers deeper research**, not a finished deliverable.
Skills produce structured files you can reference, edit, and build on in order to guide your research process. 

---

## Quick start

### 1. Clone and configure

```bash
git clone https://github.com/spm259/research-skills.git
cd research-skills
cp .env.example .env
# Add your PERPLEXITY_API_KEY for deeper web research (optional but recommended)
# arXiv and Semantic Scholar work without any key
# You can customize the skills to use other services as needed.
# You can still use the skills with no APIs.
```

### 2. Use any skill with plain English

Open Claude Code in this directory and say what you need:

```
"I want to research vertical SaaS for construction"
"Who should I talk to about embedded finance?"
"Build comps for Procore"
"Build me interview questions for a call with a VP of Product at a fintech"
"Read this paper for me: https://arxiv.org/abs/..."
```

No slash commands needed. Skills trigger on natural language.

---

## Skills

| Skill | What it does | Standalone? |
|---|---|---|
| [scope](skills/scope/SKILL.md) | Frame a research question into sub-questions and constraints | Yes |
| [source-discovery](skills/source-discovery/SKILL.md) | Find sources via WebSearch, arXiv, Semantic Scholar, Perplexity | Yes |
| [source-evaluation](skills/source-evaluation/SKILL.md) | Score sources on credibility, relevance, recency, methodology | Yes |
| [deep-read](skills/deep-read/SKILL.md) | Fetch and extract structured notes from a source | Yes |
| [synthesis](skills/synthesis/SKILL.md) | Build a landscape map across sources | Yes |
| [gap-analysis](skills/gap-analysis/SKILL.md) | Identify open questions, missing perspectives, weak evidence | Yes |
| [expert-map](skills/expert-map/SKILL.md) | Map who to talk to and how to reach them | Yes |
| [interview-questions](skills/interview-questions/SKILL.md) | Build tailored question guides for informational interviews | Yes |
| [comps](skills/comps/SKILL.md) | Build comparable companies / precedent transactions tables | Yes |
| [conversational-brief](skills/conversational-brief/SKILL.md) | Brainstorm a topic through conversation, then produce a structured one-pager | Yes |

---

## Output structure

All outputs land in `output/{topic-slug}/` (gitignored by default):

```
output/
└── {topic-slug}/
    ├── scope.md                         # Research question + sub-questions
    ├── sources/
    │   ├── discovered.md                # All sources found
    │   └── evaluated.md                 # Sources scored + prioritized
    ├── notes/
    │   └── {source-slug}.md             # One structured note per source read
    ├── synthesis.md                     # Landscape map across sources
    ├── gaps.md                          # Open questions + priority next steps
    ├── expert-map.md                    # Who to talk to
    ├── expert-map.csv                   # Clay-ready lead list
    ├── interview-questions/
    │   └── {person-slug}.md             # Question guide per interview
    ├── comps.md                         # Comparable companies / deals table
    └── brief.md                         # Conversational one-pager
```

---

## Common entry points

### Full research pipeline
```
"I want to research [topic]"
```
→ scope → source-discovery → source-evaluation → deep-read → synthesis → gap-analysis → expert-map

### Quick landscape scan (no deep reading)
```
"Find sources on X and give me a synthesis"
```
→ source-discovery → synthesis (from snippets)

### Read a single source
```
"Read this for me: [URL or arXiv ID]"
```
→ deep-read (single source mode) — returns structured notes immediately

### Investment research
```
"Build comps for [company]"
"Who are the investors in [sector]?"
"Build me interview questions for a VC who covers [sector]"
```
→ comps + expert-map (investors category) + interview-questions (investor mode)

### Prepare for a specific call
```
"I'm talking to [role] about [topic] — help me prepare"
```
→ interview-questions (standalone, no prior research needed)

### Find the gaps in what you know
```
"I've been reading about X. What am I missing?"
```
→ gap-analysis (standalone, user describes what they know)

### Capture what you already think
```
"Let's talk through X and write a brief"
"Help me think through X"
```
→ conversational-brief — Claude interviews you, then produces a one-pager from the conversation

---

## APIs used

| API | Required | Cost | Used by |
|---|---|---|---|
| Perplexity sonar-pro | No | Pay-per-use | source-discovery |
| arXiv | No | Free, no auth | source-discovery |
| Semantic Scholar | No | Free, no auth | source-discovery, source-evaluation |
| Unpaywall | No | Free (email) | deep-read |
| WebSearch / WebFetch | No | Claude Code built-in | all skills |

Skills degrade gracefully without optional APIs — see each SKILL.md for fallback behavior.

---

## Connecting skills

Skills communicate through files in `output/{topic-slug}/`. No skill calls another
directly — each reads its input files and writes its output files. This means:

- Any skill can be re-run without affecting others
- You can edit any intermediate file to steer the research
- Skills check for their input files at startup and ask you inline if they're missing

---

## Contributing

Fork this repo and adapt the skills to your workflow. The SKILL.md format is the only
convention — plain markdown files that Claude Code reads as instructions.

To add a new skill: create `skills/{skill-name}/SKILL.md` and add it to this README.
