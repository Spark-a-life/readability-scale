# /elaborate — Dual-Version Explanation Command

**Engine:** readability-scale v1.0.3

**Canonical source:** `.claude/commands/` in this repo.

When invoked, produce **two versions** of an explanation for the topic in `$ARGUMENTS` (or the most recently discussed topic if no argument is given).

This command activates **Persona 1 + Persona 2** from the readability-scale engine.
See `readability-scale.md` for persona construction rules.

---

## Output

Always produce exactly two versions, in this order:

1. **The accessible version** — analogy-first, no jargon, plain language, action-clear
2. **The technical and legal reasoning version** — expert-level, formal IDs, file paths, verification commands, governance citations

Separate with `---`. Each version must be fully self-contained.

See `readability-scale.md` for the full construction specification of each persona.

---

## Format rules

- Labels are exactly: **The accessible version** and **The technical and legal reasoning version**
- Never surface age-framing ("16-year-old", "beginner", "expert") — labels only
- If the topic spans multiple sub-items, produce both versions for each sub-item with a `---` and sub-item heading between groups
- If `$ARGUMENTS` is empty, use the most recently discussed concept in this session

---

## Compound invocation

If **`/elaborate` and `/levelset`** are both requested for the same topic in one message, follow **`readability-scale.md` — Compound invocation** (two full command outputs in request order; do not merge unless the user asks).

---

## Trigger phrases

- "explain this for better readability"
- "elaborate on this"
- "give me both versions"
- "can you elaborate"
- `/elaborate [topic]`

---

## Example invocations

```
/elaborate the retry policy
/elaborate the rate-limiting logic
/elaborate the authentication handshake
/elaborate the data retention policy
/elaborate why the cache was invalidated
/elaborate
```

---

*elaborate v1.0.3 — 2026-04-01*
*© 2026 Dr Will, Spark-a-life. Released under MIT License.*
