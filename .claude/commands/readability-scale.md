# readability-scale — Core Persona Calibration Engine

**Version:** 1.0.3 | **Date:** 2026-04-01

**Canonical source:** `.claude/commands/` in this repo.

This is the underlying engine that powers `/strip`, `/elaborate`, and `/levelset`. It defines the three audience personas and the construction rules for each version. It is not invoked directly — it is referenced by the commands above.

---

## The Three Audience Personas

### Persona 1 — The Accessible Version

**Audience:** No prior technical or legal knowledge required.
**Voice:** Plain language. Warm, direct. One idea per sentence.
**Construction rules:**
- Open with a concrete analogy drawn from everyday life (physical objects, familiar situations, things you can touch or see)
- Avoid all jargon; if a technical term must appear, define it immediately in parentheses
- Sentences are short — aim for ≤15 words each
- No acronyms unless spelled out first
- Close with one clear statement: what (if anything) the human needs to do next

**Signal phrase in output:** `**The accessible version**`

---

### Persona 2 — The Technical and Legal Reasoning Version

**Audience:** Subject-matter expert in the relevant domain (engineering, governance, law, AI systems, data protection).
**Voice:** Precise. Formal. Evidence-referenced.
**Construction rules:**
- Open with the formal problem identifier: risk register ID, control number, DPIA section, CSF reference (where one exists)
- Explain the mechanism precisely — protocols, commands, config keys, PDPA sections, control IDs, schema fields
- Reference specific files, paths, and standards that govern the decision
- State verification criteria and observable confirmation signals (commands, expected outputs)
- Where a governance or legal rationale applies, cite it explicitly (PDPA section, DPIA control ID, CSF-1/2/3, ISO standard)
- Close with the residual risk posture after the action is taken

**Signal phrase in output:** `**The technical and legal reasoning version**`

---

### Persona 3 — The Governance and Strategic Alignment Version

**Audience:** Senior executive, board member, DPO, practice lead, governance committee. Understands risk and business impact but does not operate at the technical layer.
**Voice:** Authoritative. Decision-oriented. Risk-focused.
**Construction rules:**
- Open with the strategic significance: why this decision exists and what is at risk if not addressed
- State the risk level (e.g., HIGH inherent → LOW residual) in plain terms
- Identify who is responsible and who must approve
- Quantify the cost of action vs. the cost of inaction in terms of risk exposure, compliance obligations, and business continuity — not technical detail
- Reference the governance framework that mandates the action (e.g., PDPA, DPIA conditional approval, CSF-1)
- Close with the specific decision or approval being requested, and the consequence of deferral

**Signal phrase in output:** `**The governance and strategic alignment version**`

---

## Output Assembly Rules

| Command | Personas activated | Separator |
|---------|-------------------|-----------|
| `/strip` | Persona 1 constraints applied to existing prose (reduction — no generation) | N/A |
| `/elaborate` | Persona 1 + Persona 2 | `---` between versions |
| `/levelset` | Persona 1 + Persona 2 + Persona 3 | `---` between versions |

**Universal rules (apply to `/elaborate` and `/levelset`):**
- Never surface age-framing ("16-year-old", "18-year-old", "executive") in the output — the label above each version is the only identifier
- Each version must be self-contained — a reader who reads only one version understands the full picture for their level
- If the topic spans multiple sub-items, produce all activated personas for each sub-item; separate sub-items with a full horizontal rule (`---`) with a sub-item heading above each group
- If `$ARGUMENTS` is empty, default to the most recently discussed concept in the current session

---

## Compound invocation (both commands in one session)

When the user requests **`/elaborate` and `/levelset`** on the **same topic** in one message (in any order):

- Produce **two separate command outputs** in **the order requested** — each output satisfies that command's contract in full (two labelled versions vs three labelled versions).
- **Do not merge** into a single combined response unless the user explicitly asks to combine or deduplicate.
- **Repetition is expected:** Persona 1 and Persona 2 inside `/levelset` will repeat the same layers as `/elaborate` for the same topic; that overlap is correct unless the user asks for deduplication.
- **Reading order** affects presentation only, not the definition of each command:
  - **`/elaborate` then `/levelset`** — natural build-up (operator-facing, then executive-facing).
  - **`/levelset` then `/elaborate`** — governance framing first, then dual-layer drill-down for implementers.

---

## Upgrade Path: Client Persona Types

This engine is designed to be extended. As deployments scale to institutional clients, new personas can be added or substituted for Persona 3 based on the audience:

| Client type | Persona 3 calibration |
|-------------|----------------------|
| Education institution (senior admin) | Governance obligations under MOE/MCMS; programme risk; student data protection |
| Healthcare institution (CMO/DPO) | HBRA compliance; patient data risk; clinical governance; IRB obligations |
| Corporate (Board/CHRO) | Employment law; PDPA HR data; compa-ratio risk; ISO 30414 reporting |
| Government agency | Whole-of-government AI guidelines; IMDA Agentic AI framework; public accountability |
| General (default) | Risk posture, compliance obligation, cost of inaction, approval request |

To activate a client-specific Persona 3 calibration, append the client type to the command:
```
/levelset the data retention policy --client healthcare
/levelset the pay structure --client corporate
```
*(Client-type flags are a Phase 2 upgrade — not yet implemented. Default general calibration applies in v1.0.0.)*

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0.0 | 2026-03-17 | Initial engine. Three personas. Two command surface area (`/elaborate`, `/levelset`). Client persona upgrade path documented. |
| 1.0.1 | 2026-03-25 | Single canonical copy established; sync script for workspace mirrors. |
| 1.0.2 | 2026-03-29 | Compound invocation: two full outputs when both commands requested; order preserves request sequence; no merge unless asked. |
| 1.0.3 | 2026-04-01 | Added `/strip` (Persona 1 reduction pass on existing prose). Updated Output Assembly Rules table to include all three commands. |

---

*readability-scale v1.0.3 — Core Persona Calibration Engine*
*© 2026 Dr Will, Spark-a-life. Released under MIT License.*
