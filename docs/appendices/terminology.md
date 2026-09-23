# Terminology

Terms used across this documentation, with plain definitions. Brazilian-specific
terms are given with their local name where useful.

## Temporal

| Term | Definition |
|---|---|
| **Competence** (*competência*) | The reference month that work/salary belongs to. Drives social-security, FGTS, salary-family and minimum-wage resolution. |
| **Payment date** | The date remuneration is actually paid. Drives income-tax resolution on a cash basis. |
| **Occurrence date** | The date a fact (e.g. an absence) happened; can predate the competence it applies to. |
| **Application competence** | The payroll month a fact is applied to. |
| **Effective-from / effective-to** (`VALID_TIME`) | The window during which a rule or fact is legally in force. |
| **Known-at / registered-at** (`KNOWLEDGE_TIME`) | When a rule or fact became known / was recorded. |
| **Bitemporality** | Treating effective-time and knowledge-time as independent axes. |

## Payroll domains

| Term | Definition |
|---|---|
| **DSR** (*descanso semanal remunerado*) | Weekly paid rest; can be forfeited by unjustified absence. |
| **13th salary** (*décimo terceiro*) | Statutory year-end bonus, accrued in twelfths. |
| **Vacation allowance** (*abono pecuniário*) | Sale of a portion of vacation days for cash. |
| **One-third** (*terço constitucional*) | Constitutional one-third premium on vacation. |
| **Twelfths** (*avos*) | Monthly fractions used for 13th and proportional vacation. |
| **Frequency** (*frequência*) | Attendance facts: absence, delay, early departure. |
| **Salary substitution** (*salário-substituição*) | Pay for temporarily filling another role. |
| **Worker credit / consignado** | Payroll-deductible loan, limited to a margin over available remuneration. |
| **Retroactive / supplementary payroll** (*folha complementar*) | A run paying a retroactive difference without rewriting the closed original. |

## Fiscal / compliance

| Term | Definition |
|---|---|
| **INSS** | Social-security contribution (progressive brackets, ceiling). Resolved by competence. |
| **IRRF** | Withheld income tax. Resolved by payment date (cash basis). |
| **FGTS** | Severance fund; an employer deposit (typically 8% of base). Resolved by competence. |
| **Salary-family** (*salário-família*) | Allowance per eligible dependent below an income ceiling. |
| **Legal package** | Versioned federal fiscal parameters, resolved by effective date. |
| **Collective agreement** (*CCT/ACT*) | Sector or company-level bargaining instrument. |
| **Addendum** (*aditivo*) | An amendment to a collective instrument, applied as a delta. |
| **No silent fallback** | The posture of raising an explicit coverage error rather than applying a wrong rule. |
| **eSocial** | The government digital bookkeeping system for labor/fiscal events. |
| **Restricted production** (*produção restrita*) | The government homologation environment; not production. |

## Engine / architecture

| Term | Definition |
|---|---|
| **Contractual facts** | The five temporal facts: remuneration; role/function/CBO; workweek; contract type/duration; dependents. |
| **Canonical payroll history** | The temporal model of the employment relationship the engine consumes. |
| **Immutable snapshot** | A versioned, frozen record of a closed competence. |
| **Execution envelope** | The recorded context (competence, payment date, resolved packages) of a calculation. |
| **Replay capsule** | Minimal, PII-free evidence to reproduce/analyze a result or divergence. |
| **Resolution trace** | The step-by-step record of how a rule was resolved (national → collective → final). |
| **Master Shadow** | The internal longitudinal validation protocol (engine vs independent oracle). |
| **Oracle** | An independent implementation of the rules used to check the engine. |
| **Trusted-since** | The point from which a fact's provenance is trusted; earlier competences return review-required. |
