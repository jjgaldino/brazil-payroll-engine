# Legal Package

The legal package resolves federal fiscal parameters — social security (INSS),
income tax (IRRF), salary-family, minimum wage and the severance fund (FGTS) — by
**effective date**, on a declared temporal basis, with **no silent fallback**.

## Purpose

Provide the calculators with the correct fiscal parameters for a given competence
and payment date, versioned by effective date, and fail loudly when coverage is
missing instead of applying a nearby-but-wrong rule.

## Structure

Each parameter family is versioned with an effective range and a source reference
(the norm that established it). The resolver declares, per family, the **temporal
basis** on which it is resolved:

| Family | Temporal basis | Rationale |
|---|---|---|
| Social security (INSS) | **Competence** | Contribution belongs to the work month. |
| Income tax (IRRF) | **Payment date** | Cash-basis taxation; the table can move intra-year. |
| Salary-family | **Competence** | Benefit belongs to the work month. |
| Minimum wage | **Competence** | Reference for the work month. |
| Severance fund (FGTS) | **Competence** | Deposit belongs to the work month. |

The output of a resolution names the family, the temporal basis, the reference date
that selected the package, the resolved version, its effective range, and the legal
source — a fully traceable envelope.

## Effective-date semantics

- **Competence-based families** are resolved by the work month.
- **Income tax** is resolved by the **payment date**. This matters: an August
  competence paid in early September uses the income-tax table in force in
  **September**. If a collective agreement shifts the payment date, the income-tax
  resolution follows — closing the loop with the
  [calendar & payment schedule](#relationship-to-the-calendar).

## No silent fallback

This is a defining posture. When the resolver cannot find coverage, it raises an
explicit error rather than substituting a wrong rule:

- **Coverage missing** — no package exists for the requested year/vigency (or the
  request falls in a gap between vigencies). The engine returns a coverage error
  that names the available coverage; it does **not** apply the nearest year.
- **Temporal reference missing** — income tax requires a payment date; if one is not
  supplied, the resolver errors rather than substituting the competence.

The consequence is that an unmodeled period **stops** the calculation loudly, which
is exactly what prevents latent, compounding fiscal errors.

## Coverage

The package covers the fiscal parameters needed for the validated horizon
(multi-year INSS brackets and ceiling, income-tax vigencies including intra-year
changes, salary-family value and eligibility ceiling, minimum wage, and the FGTS
rate with category exceptions). Newer regimes are staged where the norms are
published (for example, the consumption-tax reform milestones are staged by year).

Because coverage is finite by design, extending it forward (a new year, a new
vigency) is a **parameter update** — a versioned addition — not an engine change.
See [regulatory watch](regulatory-watch.md).

## Relationship to the calendar

The income-tax basis depends on the payment date, which is resolved by the
[calendar & payment schedule](regulatory-watch.md) (day semantics and payment-nature
rules). The sequence is: competence resolves the competence-based families; the
schedule resolves the payment date; the payment date resolves income tax.

## Validation status

`VALIDATED`. The strict, effective-date resolution — including the income-tax
payment-date basis and the absence of silent fallback — participated in the
longitudinal protocol. A validation finding confirmed the payment-date basis for
income tax on vacation (the engine resolved by payment date; an independent oracle
that resolved by year was corrected). See
[validation overview](../05-validation/validation-overview.md).

## Known boundaries

- Coverage is finite and forward-bounded: future years/vigencies must be added as
  parameter updates before they can be resolved (and until then, they error rather
  than guess).
- International tax-treaty situations are out of the current scope.
