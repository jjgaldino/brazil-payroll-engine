# Functional Coverage

*Public coverage matrix. Status reflects the engine's implemented and internally
validated scope. This is not a third-party certification, and it does not claim
to cover every Brazilian payroll scenario.*

## Status legend

| Status | Meaning |
|---|---|
| `VALIDATED` | Implemented **and** exercised under the longitudinal internal validation protocol (engine vs independent oracle) and/or a dedicated golden-test suite, with zero unexplained divergence in the tested scope. |
| `IMPLEMENTED` | Implemented and covered by internal tests, but not (yet) longitudinally validated as an isolated domain. |
| `PARTIALLY_VALIDATED` | Core paths validated; some sub-cases remain implemented-only or scoped to specific instruments/categories. |
| `PLANNED` | Modeled or scoped, not yet implemented as a calculating domain. |
| `KNOWN_BOUNDARY` | Explicitly outside the current validated scope (documented limit, not a hidden gap). |

## Coverage matrix

### Core monthly payroll & fiscal bases

| Domain | Status | Notes |
|---|---|---|
| Monthly payroll | `VALIDATED` | Deterministic engine; longitudinal monthly backbone validated across 24 competences. |
| INSS (progressive) | `VALIDATED` | Per-competence brackets, ceiling, multi-employment ceiling sharing. |
| IRRF | `VALIDATED` | Payment-date (cash-basis) resolution; legal vs simplified deduction; monthly reduction (2026+). |
| FGTS | `VALIDATED` | 8% base with category exceptions; maintained during qualifying leaves. |
| Salary-family | `VALIDATED` | Dependent count + eligibility ceiling; prorated on partial months. |
| Admission (proration + event) | `IMPLEMENTED` | Mid-month proration validated in monthly flow; eSocial S-2200 event built. |

### Time, absence & vacation

| Domain | Status | Notes |
|---|---|---|
| Frequency (absence / delay / early departure) | `VALIDATED` | Temporal facts that reduce base semantically; sourced, never inferred. |
| DSR (weekly rest) | `VALIDATED` | Weekly forfeiture on unjustified absence; calendar-based on variables. |
| Vacation | `VALIDATED` | Acquisition (absence table), value + 1/3, competence split, payment-date IRRF. |
| Vacation allowance (abono) | `PARTIALLY_VALIDATED` | INSS/FGTS-exempt; 1/3-on-allowance is income-tax-taxable per tax authority guidance. |
| Leaves | `VALIDATED` | Taxonomy (interruption/suspension), company vs social-security days, reflections. |
| Accident leave (FGTS maintained) | `VALIDATED` | FGTS kept on full base with reduced salary — a permanent proof case. |
| Maternity leave | `IMPLEMENTED` | Payment responsibility + contribution rules modeled and versioned. |

### Annual & event payroll

| Domain | Status | Notes |
|---|---|---|
| 13th salary | `VALIDATED` | Avos (≥15-day month), fixed + variable components, per-installment FGTS rounding, provisional→definitive. |
| Termination | `VALIDATED` | Rights matrix by cause; consumes full history; immutable snapshot; eSocial S-2299. |
| Retroactive / supplementary payroll | `VALIDATED` | Collective-raise back pay per originating competence; original stays immutable. |
| Overtime | `VALIDATED` | Hour value × collective rate + DSR reflection. |
| Night premium | `PARTIALLY_VALIDATED` | Collective-overridden rate applied; validated within the covered instruments. |
| Salary substitution | `VALIDATED` | Substitution pay + interaction with worker credit validated. |

### Compliance & obligations

| Domain | Status | Notes |
|---|---|---|
| Legal package (versioned parameters) | `VALIDATED` | Strict, effective-date resolution with no silent fallback. |
| Collective rule package | `PARTIALLY_VALIDATED` | Full model + one real instrument-set fully implemented; broad universe is a known boundary. |
| Worker credit / consignado | `VALIDATED` | Margin over *available remuneration* (never a flat % of base); validated on synthetic + real-data probe. |
| Salary-family / dependents (temporal) | `VALIDATED` | Dependent eligibility resolved by validity period. |
| eSocial (build/harden/validate/transmit) | `PARTIALLY_VALIDATED` | Events built, hardened, schema-checked; end-to-end exercised against **restricted-production (homologation)**, not production. |
| Regulatory watch | `IMPLEMENTED` | Signal model + classification + dedup; automated source ingestion is `PLANNED`. |
| Consumption-tax reform (CBS/IBS) | `PLANNED` | Parameter milestones staged (2026 / 2027 / 2033); calculation staging only. |

### Specialized regimes

| Domain | Status | Notes |
|---|---|---|
| Intermittent worker | `IMPLEMENTED` | Per-period remuneration, DSR, 13th, vacation, social security, termination modeled. |
| Apprentice | `IMPLEMENTED` | Reduced FGTS rate and contract-type constraints. |
| Multiple employment (INSS ceiling sharing) | `IMPLEMENTED` | Shared social-security ceiling across concurrent contracts. |
| Judicial deductions (alimony/garnishment) | `IMPLEMENTED` | Versioned judicial domain; cannot be bypassed via the manual channel. |
| Collective vacation | `IMPLEMENTED` | Planning + financial + eSocial bridge. |
| Fixed-term contracts | `IMPLEMENTED` | Early-termination indemnities and assecuratory-clause handling. |

## How to read this matrix

- `VALIDATED` means the domain participated in the longitudinal engine-vs-oracle
  protocol and/or has a dedicated golden suite that passed with zero unexplained
  divergence **within the tested scope** — not that every conceivable case is
  covered.
- `KNOWN_BOUNDARY` items are documented deliberately. The most important one is
  that the collective-bargaining universe is vast; the engine implements a complete,
  auditable *model* and has fully implemented the instruments used in validation,
  but has not implemented every Brazilian agreement.

See the per-domain deep dives in [`../03-payroll-domains/`](../03-payroll-domains/)
and the [public evidence matrix](../appendices/evidence-matrix-public.md).
