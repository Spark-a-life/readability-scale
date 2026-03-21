# readability-scale

A Claude Code slash-command pattern that produces calibrated explanations at multiple audience levels from a single prompt.

## What it does

Two commands, one engine:

| Command | Versions produced | Best for |
|---|---|---|
| `/elaborate [topic]` | Accessible + Technical/Legal | Technical peers, operators |
| `/levelset [topic]` | Accessible + Technical/Legal + Governance/Strategic | When a decision-maker, sponsor, or board member is in the room |

Each version is fully self-contained — a reader who reads only one version understands the full picture for their level.

## Install

Copy the three files into your project's `.claude/commands/` folder:

```
.claude/
└── commands/
    ├── readability-scale.md   ← engine (referenced by commands, not invoked directly)
    ├── elaborate.md           ← /elaborate
    └── levelset.md            ← /levelset
```

## Usage

```
/elaborate the retry policy
/levelset the data retention policy
/levelset the authentication model --client healthcare
```

If no argument is given, both commands default to the most recently discussed concept in the session.

## The three personas

| Version | Audience | Voice |
|---|---|---|
| **The accessible version** | No prior technical knowledge required | Analogy-first, plain language, ≤15 words/sentence |
| **The technical and legal reasoning version** | Subject-matter expert | Precise, formal, evidence-referenced, file paths and control IDs |
| **The governance and strategic alignment version** | Executive / board / DPO | Risk posture, cost of inaction, compliance obligations, approval request |

The labels above are the stable public interface. Age-framing ("16-year-old", "executive") is never surfaced in output.

## Adapting Persona 3 for your context

`readability-scale.md` documents an upgrade path for client-specific Persona 3 calibrations (education, healthcare, corporate, government). Replace the sector-specific governance frameworks in that table with your own standards.

## Origin

A community contribution. Adapted from an internal governance-driven AI workflow system.

© 2026 Dr William Jing Wen Siew. Released under MIT License.
