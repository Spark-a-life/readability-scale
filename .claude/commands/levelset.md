# /levelset — Triple-Version Explanation Command

**Engine:** readability-scale v1.0.3

**Canonical source:** `.claude/commands/` in this repo.

When invoked, produce **three versions** of an explanation for the topic in `$ARGUMENTS` (or the most recently discussed topic if no argument is given).

This command activates **Persona 1 + Persona 2 + Persona 3** from the readability-scale engine.
See `readability-scale.md` for persona construction rules and the client persona upgrade path.

---

## Output

Always produce exactly three versions, in this order:

1. **The accessible version** — analogy-first, no jargon, plain language, action-clear
2. **The technical and legal reasoning version** — expert-level, formal IDs, file paths, verification commands, governance citations
3. **The governance and strategic alignment version** — executive-level, risk posture, compliance obligations, cost of inaction, approval request

Separate each with `---`. Each version must be fully self-contained.

See `readability-scale.md` for the full construction specification of each persona.

---

## Format rules

- Labels are exactly as shown above — no deviation
- Never surface age-framing or persona-type labels ("16-year-old", "executive", "beginner") — the section heading is the only identifier
- If the topic spans multiple sub-items, produce all three versions for each sub-item with a `---` and sub-item heading between groups
- If `$ARGUMENTS` is empty, use the most recently discussed concept in this session
- To activate a client-specific calibration for Persona 3, append `--client <type>`:
  ```
  /levelset the data retention policy --client healthcare
  /levelset the pay structure --client corporate
  ```
  *(Client-type flags are a Phase 2 upgrade — general calibration applies in v1.0.0)*

---

## Compound invocation

If **`/elaborate` and `/levelset`** are both requested for the same topic in one message, follow **`readability-scale.md` — Compound invocation** (two full command outputs in request order; do not merge unless the user asks).

---

## Trigger phrases

- "levelset this"
- "can you levelset"
- "give me all three versions"
- "explain this at all levels"
- `/levelset [topic]`

---

## Example invocations

```
/levelset the rate-limiting logic
/levelset the data retention policy
/levelset why the cache was invalidated
/levelset the authentication handshake
/levelset
/levelset the observability stack --client healthcare
/levelset the data retention policy --client healthcare
```

---

## When to use /levelset vs /elaborate

| Use `/elaborate` when... | Use `/levelset` when... |
|--------------------------|------------------------|
| Your audience is technical staff or operators | Your audience includes a decision-maker, sponsor, or board member |
| The topic is implementation-focused | The topic has governance, risk, or approval implications |
| A two-level explanation is sufficient | You need executive sign-off or stakeholder alignment |

---

## Community note

This command is a system-agnostic pattern built on the readability-scale engine. The three output labels are the stable public interface. The internal construction method is defined in `readability-scale.md` and is intentionally not surfaced in output.

The engine is designed to be upgraded with client persona-types (education, healthcare, government, corporate). See the upgrade path in `readability-scale.md`.

If you are adapting this for your own system, replace any system-specific trigger phrases, escalation paths, and client-type flags with your own equivalents. The three-persona structure applies universally.

---

*levelset v1.0.3 — 2026-04-01*
*© 2026 Dr Will, Spark-a-life. Released under MIT License.*
