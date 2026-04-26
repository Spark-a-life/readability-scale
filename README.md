# readability-scale

A Claude Code slash-command pattern that strips, explains, and calibrates prose at multiple audience levels from a single prompt.

## What it does

Three commands, one engine:

| Command | Operation | Output | Best for |
|---|---|---|---|
| `/strip [prose]` | Reduction — existing text → clearest form | One stripped version | Drafts that are too dense, passive, or bury the finding |
| `/elaborate [topic]` | Generation — topic → dual-audience | Accessible + Technical/Legal | Technical peers, operators |
| `/levelset [topic]` | Calibration — topic → tri-audience | Accessible + Technical/Legal + Governance/Strategic | When a decision-maker, sponsor, or board member is in the room |

Each command moves in a distinct direction. `/strip` reduces prose you already have — it does not generate new content. `/elaborate` and `/levelset` generate multi-audience versions from a topic.

Each generated version is fully self-contained — a reader who reads only one version understands the full picture for their level.

## Install

Copy the four files into your project's `.claude/commands/` folder:

```
.claude/
└── commands/
    ├── readability-scale.md  ← engine (referenced by commands, not invoked directly)
    ├── strip.md              ← /strip
    ├── elaborate.md          ← /elaborate
    └── levelset.md           ← /levelset
```

## Usage

```
/strip [paste draft prose here]
/elaborate the retry policy
/levelset the data retention policy
/levelset the authentication model --client healthcare
```

If no argument is given, `/elaborate` and `/levelset` default to the most recently discussed concept in the session. `/strip` defaults to the most recently produced prose.

## The three personas

| Version | Audience | Voice |
|---|---|---|
| **The accessible version** | No prior technical knowledge required | Analogy-first, plain language, ≤15 words/sentence |
| **The technical and legal reasoning version** | Subject-matter expert | Precise, formal, evidence-referenced, file paths and control IDs |
| **The governance and strategic alignment version** | Executive / board / DPO | Risk posture, cost of inaction, compliance obligations, approval request |

The labels above are the stable public interface. Age-framing ("16-year-old", "executive") is never surfaced in output.

## `/strip` — what it does and doesn't do

`/strip` applies Persona 1 constraints to existing prose in a single model pass:
- Sentences ≤15 words
- Contribution in sentence 1
- Active voice throughout
- No jargon, no filler, no nominalisations
- Logical connectives, hedges, and sequential markers are preserved (they carry argumentative structure — stripping them destroys the claim)

It is not Claude Code's built-in `/simplify` command, which is an Anthropic-authored code-quality agent that operates on recently modified code files. `/strip` operates on prose only, in one pass, with no agents.

## Adapting Persona 3 for your context

`readability-scale.md` documents an upgrade path for client-specific Persona 3 calibrations (education, healthcare, corporate, government). Replace the sector-specific governance frameworks in that table with your own standards.

## Origin

A community contribution. Adapted from an internal governance-driven AI workflow system.

© 2026 Dr Will, Spark-a-life. Released under MIT License.
