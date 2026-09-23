# Retroactive & Supplementary Payroll

When a collective raise (or other rule change) becomes effective retroactively, the
engine pays the difference **without rewriting** the closed originals. This is the
practical expression of the [temporal model](../02-architecture/temporal-model.md).

## Purpose

Compute back pay when a rule (typically a collective wage adjustment) is effective
in the past but becomes known later, producing a **supplementary payroll** that pays
the difference per originating competence while the original closed payrolls remain
immutable.

## Inputs

- The new salary rule (percentage and/or floor) and its **effective-from** date.
- The employee's **salary history** across the affected period (so each month uses
  its real base, not a flat delta).
- The competences affected (respecting effective dates, admission and termination),
  the days actually due in each, and the payment date of the supplementary run.

## Outputs

- The **salary difference per competence**, computed on each month's real base and
  prorated by the days due.
- The **statutory reflections** of the difference (overtime, night premium, and the
  vacation/13th averages) recomputed through the certified calculators.
- The correct **tax treatment**: social security recomposed per competence; income
  tax applied under the regime appropriate to prior-year vs current-year amounts.
- A **supplementary record** linked to the original, never a mutation of it.

## Temporal behavior

- The original competence's snapshot stays **immutable**; the supplementary run is a
  new, linked record referencing it.
- The difference is **not** `newSalary − oldSalary` applied bluntly. Each affected
  month is recomputed on its own base (carrying salary history forward), adjusted by
  the rule, and prorated by days due.
- Retroactivity is **clamped** to the rule's effective-from date; the engine does
  not retroact before it.
- **Income tax** distinguishes prior-year amounts (a distinct regime, using the
  payment-month table with the appropriate treatment) from current-year amounts.

## Compliance dependencies

- Collective-bargaining provisions on retroactive adjustments.
- Tax rules on the treatment of amounts relating to prior periods.
- The rule that an adjustment does not reduce salary — the difference is
  non-negative.

## Auditability

Each competence's difference traces to the originating snapshot and the rule that
produced it. Reflections are recomputations through the same certified calculators
used for the regular payroll, not new formulas. The three dates — originating
competence, date known, and payment date — are all carried, which is what allows the
correct tax treatment and a full audit trail.

## Validation status

`VALIDATED`. Retroactive/supplementary payroll passed the longitudinal protocol
across two modeled adjustments (one known in late one year, one in another), each
spanning multiple competences, with the differences, reflections and taxes matching
the independent oracle to the cent. Bitemporality — closed originals immutable,
differences recognized per originating competence — is a proven property.

## Known boundaries

- Reflections into earnings whose rules are not automatically altered by an
  adjustment (for example certain allowances) are **not** applied automatically;
  they require an explicit rule change.
- Multiple-employment scenarios compute each contract in isolation and then
  aggregate; there is no cross-employer retroactive linkage.
