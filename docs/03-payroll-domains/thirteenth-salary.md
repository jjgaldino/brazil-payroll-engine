# 13th Salary

The 13th-salary (year-end bonus) domain computes the twelfths earned, the fixed and
variable components, the installment split, and the reconciliation from a
provisional to a definitive figure.

## Purpose

Compute the statutory year-end bonus: the number of twelfths earned across the year
(each qualifying month counts), the fixed component from the reference salary and
the variable component from averages, the split into installments with correct
social-security, income-tax and severance-fund treatment, and the adjustment from a
provisional (through November) to a definitive (with December) amount.

## Inputs

- The **year**, admission and (if any) termination dates, and the employee's leaves
  during the year.
- The **reference salary** (December) and the **history of variable earnings** for
  the variable component's averages.
- The advance already paid (typically the first installment) and dependents for
  income tax.

## Outputs

- The **twelfths** earned (0–12) with the qualifying months.
- The **gross** bonus (fixed + variable components).
- The **installments**: the advance (first) and the second, with social security
  and income tax applied on the appropriate bases, and the **per-installment**
  severance-fund accrual.
- The provisional-vs-definitive **adjustment** where applicable.

## Temporal behavior

- **Twelfths** are counted per calendar month, with a month counting as a full
  twelfth once a threshold of days is met. Admissions, terminations and leaves
  adjust the qualifying months.
- The **fixed component** uses the December reference salary; the **variable
  component** uses the year's averages — provisional (through November) or
  definitive (with December).
- A **provisional** figure computed before December is later reconciled to the
  **definitive** figure; the difference is carried as a dedicated complement.

## Compliance dependencies

- The statutory 13th-salary provisions (twelfths, installments, severance-fund
  deposit).
- The rule that a qualifying month is counted as a full twelfth once the day
  threshold is met.
- Guidance that accident-related absences do not reduce the bonus within the same
  period.

## Auditability

The twelfths, the fixed and variable components, and each installment's bases are
exposed. A specific, deliberately preserved property: the **severance fund is
deposited per installment, each rounded**, and the two per-installment deposits sum
to the whole — an accounting-coherence invariant. (This exact rounding behavior was
the subject of a validation finding in which the engine was shown to be correct and
an independent oracle was corrected — see
[validation overview](../05-validation/validation-overview.md).)

## Validation status

`VALIDATED`. The 13th-salary domain passed the longitudinal protocol across the
tested year-ends and has a dedicated golden suite covering the twelfths threshold,
the fixed-plus-variable components, the installment split, and the per-installment
severance rounding.

## Known boundaries

- Twelfths follow the day-threshold rule strictly; there is no interpolation below
  the threshold.
- The variable component depends on the history supplied; incomplete history is
  handled by the provisional/definitive mechanism, not by guessing.
