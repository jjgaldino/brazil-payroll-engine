# Monthly Payroll

The monthly payroll calculator is the spine of the engine. It turns an employee's
resolved contractual facts and the applicable rules into a complete payslip and
the derived tax bases.

## Purpose

Produce, for one employee and one competence, a complete set of payslip events
(earnings, deductions, informational lines) and the derived bases and totals:
social-security base and withholding (INSS), income-tax base and withholding
(IRRF), severance-fund base and accrual (FGTS), salary-family allowance, and net
pay. The calculator is a **pure function** — no I/O, no ambient state — which is
what enables deterministic replay.

## Inputs

- The **employee record** (resolved for the competence): salary and salary type
  (monthly, hourly, daily, commission, mixed), workweek, dependents for income tax,
  category, admission/termination dates.
- **Days in the month** (calendar days, 28–31).
- **Current-period facts**: frequency occurrences, leaves, variable events
  (overtime, night premium, commissions), consigned-credit installments,
  salary-family dependents, hour-bank settlements, mid-month salary changes.
- **Temporal keys**: the competence (drives INSS/FGTS/salary-family/minimum wage)
  and the payment date (drives income tax on a cash basis).
- **Resolved rules**: the legal package (fiscal parameters) and any collective
  overrides (workweek/divisor, premium rates).

## Outputs

- A list of **events** — each with a code, description, type (earning/deduction/
  informational), a reference (days, hours, dependents, or aliquot), an amount, and
  incidence flags (whether it enters the INSS, income-tax, and FGTS bases).
- **Derived bases**: INSS base, income-tax base, FGTS base.
- **Withholdings and accruals**: INSS withheld, income tax withheld, FGTS accrued,
  salary-family allowance.
- **Totals**: total earnings, total deductions, net pay.
- An optional **calculation trace** (see *Auditability*).

## Temporal behavior

Two temporal axes are honored:

- **Competence** resolves social-security, FGTS, salary-family and minimum-wage
  parameters — the month the work belongs to.
- **Payment date** resolves the income-tax table on a **cash basis**; it can differ
  from the competence and shift intra-year.

Admissions and terminations **prorate** salary, salary-family and bases by the days
actually worked. Mid-month salary changes are applied by validity period. The DSR
(weekly paid-rest) reflection over variable earnings uses the **actual calendar** of
the month (Sundays and holidays), not a fixed approximation.

## Compliance dependencies

- Federal fiscal parameters (INSS brackets and ceiling, income-tax brackets and
  deductions, salary-family value and eligibility ceiling, minimum wage, FGTS rate)
  — resolved from the [legal package](../04-compliance/legal-package.md).
- Collective overrides where applicable (workweek/divisor and premium rates) — from
  the [collective rule package](../04-compliance/collective-rule-package.md).
- The [calendar & payment schedule](../04-compliance/legal-package.md) for the
  payment-date basis of income tax.

## Auditability

Every calculation can be explained. The trace exposes:

- **INSS** — the base, the ceiling, and the per-bracket breakdown that sums to the
  withholding.
- **Income tax** — the gross base, the deductions considered (social security,
  dependents, and the simplified discount), which deduction was used, the applied
  bracket, and any monthly reduction.
- **Bases by rubric** — which earnings entered each base.

Each event records exactly which rubric produced which amount under which
incidence, so no aggregation is lossy and the result can be replayed from its
inputs.

## Validation status

`VALIDATED`. The monthly backbone participated in the longitudinal engine-vs-oracle
protocol across 24 competences and is covered by a dedicated golden-test suite
spanning prorations (admission/termination on day 1, mid-month, last day),
mid-month salary changes, bracket boundaries and ceilings, category exceptions, and
the interaction of frequency facts with the bases. See
[validation overview](../05-validation/validation-overview.md).

## Known boundaries

- **Below-minimum remuneration is not silently topped up.** When remuneration falls
  below the minimum wage (for example under specific multi-employment situations),
  the engine warns rather than assuming an employer complement — because the
  correct treatment depends on facts the engine may not hold.
- **Manual channel cannot carry judicial deductions.** Alimony/garnishment and
  similar judicial amounts must come from the versioned judicial domain; they are
  rejected if injected as manual events. See
  [security overview](../07-security/security-overview.md).
- The set of specialized categories with **longitudinal** (as opposed to unit-test)
  validation is finite; see the [functional coverage](../01-overview/functional-coverage.md)
  matrix.
