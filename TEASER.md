# Ordo Payroll Core — Technical Teaser

*Pre-NDA overview. Documentation only; no source code or confidential data.*

---

## The asset in one paragraph

Ordo Payroll Core is a **Brazilian payroll and compliance engine** that resolves
statutory and collective-bargaining rules **by validity period** and computes the
full set of Brazilian payroll obligations — monthly payroll, vacation, 13th
salary, terminations, leaves, frequency/DSR, overtime, retroactive/supplementary
payroll, and eSocial events — from a **canonical history of contractual facts**.
Its differentiators are a **bitemporal** rule model (effective-time vs
knowledge-time), a **no-silent-fallback** compliance posture, and a **longitudinal
validation** protocol that compared the engine against an independent oracle over
24 consecutive months with zero final divergences.

## Why this is hard (and why it matters)

Brazilian payroll correctness is a *temporal* problem, not an arithmetic one:

- The right INSS/FGTS table depends on the **competence** (work month).
- The right income-tax (IRRF) table depends on the **payment date** (cash basis),
  which can move intra-year.
- The right wage floor / overtime / night premium depends on the **collective
  agreement** in force for that worker.
- A collective raise agreed *today* but effective *months ago* must be paid
  **without rewriting** already-closed payrolls.

Most systems bury these decisions in code. Ordo Payroll Core makes them **explicit,
versioned, and auditable**, and fails **loudly** (coverage error) when a rule is
missing instead of silently applying the wrong one.

## What has been validated

An internal "Master Shadow" protocol ran the **production engine** against a
**separately implemented oracle**, month by month:

| Metric | Result |
|---|---|
| Consecutive competences | 24 (2024-09 → 2026-08) |
| Synthetic personas | 6 |
| Payroll domains | 10 |
| Engine-vs-oracle validations | 185 |
| Final divergences | 0 |
| Replay | Deterministic |
| Persisted snapshots (isolated staging) | 24/24, with DB reload + process-restart replay |

Two discrepancies found during validation were traced to the **oracle**, not the
engine — the engine was vindicated in both — and are documented openly. This is an
**internal** protocol, not a third-party certification.

## Integration shape

```
ERP / HCM / HRTech
      ↓  (canonical payroll history + current facts)
Ordo Payroll Core
      ↓
payroll result · tax bases · employer charges · vacation · 13th ·
termination · eSocial events · audit trace · replay data
```

A buyer can integrate **only the engine**, keeping their own front office.

## Possible transaction structures

Technology/IP acquisition · exclusive license · non-exclusive OEM · white-label ·
hosted payroll API · dedicated deployment. (No pricing or definitive terms here.)

## What is *not* claimed

- Not "all Brazilian payroll": the validated scope is finite and honest.
- No shipped connectors for every legacy vendor; the integration surface is a
  canonical model.
- No external third-party certification has been performed.

## Next step

A private data room (product & technology dossier, validation dossier,
integration dossier, IP inventory, detailed evidence matrix) is available under
NDA for technical due diligence.

Contact: `COMMERCIAL_CONTACT_TO_BE_DEFINED`.

---

*Copyright © 2026 Inventare Solutions LTDA. All rights reserved.*
