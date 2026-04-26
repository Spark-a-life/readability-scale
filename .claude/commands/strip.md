# /strip — Prose Stripping Command

**Engine:** readability-scale v1.0.3 | Persona 1 reduction pass

**Canonical source:** `.claude/commands/` in this repo.

---

## What This Command Does

`/strip` takes existing prose and returns a simplified version by applying
Persona 1 constraints from the readability-scale engine.

This is a **reduction operation on existing text** — not a generation command.
It does not produce multiple audience versions. It produces one: the clearest,
most direct form of the input prose, in a single model pass.

**No agents are spawned. No subprocesses are invoked. One pass.**

**Usage:**
```
/strip [prose block]
/strip                   ← defaults to most recently produced prose in session
```

---

## Stripping Rules (Persona 1 Constraints)

Apply all of the following to the input prose:

| Rule | Instruction |
|------|-------------|
| Sentence length | ≤15 words per sentence — split any sentence that exceeds this |
| Contribution first | The finding or contribution must appear in sentence 1 — restructure if buried |
| Active voice | Convert passive constructions to active throughout |
| No jargon | Replace technical terms with plain equivalents; if the term must be kept, define it in parentheses on first use |
| No nested clauses | Flatten compound and embedded subordinate clauses into separate sentences |
| One idea per sentence | Split sentences carrying more than one idea |
| No filler | Remove: "it is important to note", "in order to", "it can be seen that", "it is worth noting", "given the fact that", "as a result of", "with regard to" — and all equivalents |
| No nominalisations | Convert noun-heavy constructions back to verb form where clearer ("conduct an investigation" → "investigate") |

---

## Balance — Do Not Over-Strip

Stripping has a floor. Stop before the prose loses its logical architecture:

| Do not remove | Why |
|---|---|
| Logical connectives (however, therefore, because, although) | These carry argumentative structure — removing them destroys the claim |
| Sequential markers (first, then, finally) | These signal method steps — removing them collapses ordered reasoning |
| Qualifying hedges on uncertain claims (may, suggests, is consistent with) | These are epistemic precision, not filler — removing them turns inference into assertion |
| Paragraph breaks | Separate ideas stay separate — do not merge paragraphs that cover distinct points |

**Clarity over brevity.** If splitting a sentence creates ambiguity or severs the logical connection between two ideas, keep the original sentence structure and flag: `[KEPT — logical connection]`.

---

## Immutable Constraints

The following must be preserved verbatim — do not rephrase, abbreviate, or reorder:

- Numerical values and statistical annotations (p-values, CIs, n, effect sizes)
- Author names, citation keys, and figure references
- Classification labels and governance terms specific to your system
- Any term your system designates as protected

---

## Output Format

- Return the stripped prose **only** — no preamble, no explanation of what changed
- Preserve paragraph breaks and section headings from the original
- Inline flags (do not list at the end):
  - `[KEPT — factual precision]` — sentence preserved because simplification would lose accuracy
  - `[KEPT — logical connection]` — sentence preserved because splitting would sever argumentative structure
- If the input contains multiple sections, strip each separately and preserve section headings

---

## What This Command Is Not

- **Not `/elaborate`** — that generates dual-audience versions from a topic; `/strip` reduces existing prose
- **Not `/levelset`** — that generates triple-audience versions from a topic; `/strip` reduces existing prose
- **Not Claude Code's `/simplify`** — that is an Anthropic-authored code-quality agent (operates on recently modified code files, spawns parallel sub-agents). `/strip` is a single-pass prompt engineering command for prose. No agents. No code review.

---

## Document Pipeline Integration

`/strip` applies at any stage where prose has been drafted and needs to be readable before it exits:

| Stage | Apply `/strip` to |
|---|---|
| Abstract / executive summary | Opening sentences — contribution must land in sentence 1 |
| Introduction / background | Opening paragraph — strip before the body |
| Results / findings | Each paragraph before it is attached to a figure or table |
| Response text | Peer-review or stakeholder responses before sending |

Run `/strip` after drafting a section, before emitting or publishing it.

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-04-01 | Initial command. Named `/strip` to avoid collision with Claude Code's bundled `/simplify` (code-quality agent). Persona 1 reduction pass. Balance floor added. |

---

*strip v1.0.0 — 2026-04-01*
*© 2026 Dr Will, Spark-a-life. Released under MIT License.*
