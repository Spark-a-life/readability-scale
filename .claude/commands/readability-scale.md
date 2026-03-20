# readability-scale — Core Persona Calibration Engine

**Version:** 1.0.0 | **Date:** 2026-03-17

This is the underlying engine that powers `/elaborate` and `/levelset`. It defines the three audience personas and the construction rules for each version. It is not invoked directly — it is referenced by the commands above.

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
| `/elaborate` | Persona 1 + Persona 2 | `---` between versions |
| `/levelset` | Persona 1 + Persona 2 + Persona 3 | `---` between versions |

**Universal rules (apply to both commands):**
- Never surface age-framing ("16-year-old", "18-year-old", "executive") in the output — the label above each version is the only identifier
- Each version must be self-contained — a reader who reads only one version understands the full picture for their level
- If the topic spans multiple sub-items, produce all activated personas for each sub-item; separate sub-items with a full horizontal rule (`---`) with a sub-item heading above each group
- If `$ARGUMENTS` is empty, default to the most recently discussed concept in the current session

---

## Upgrade Path: Client Persona Types

This engine is designed to be extended. New personas can be added or substituted for Persona 3 based on the audience:

| Client type | Persona 3 calibration |
|-------------|----------------------|
| Education institution (senior admin) | Programme governance obligations; student data protection; accreditation risk |
| Healthcare institution (CMO/DPO) | Patient data compliance; clinical governance; ethics board obligations |
| Corporate (Board/CHRO) | Employment law; HR data privacy; compensation risk; workforce reporting standards |
| Government agency | Public accountability; national AI governance guidelines; procurement compliance |
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

---

*readability-scale v1.0.0 — Core Persona Calibration Engine*
*© 2026 Dr Will, APE Intelligence. All rights reserved.*
