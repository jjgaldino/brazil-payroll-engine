# Vacation

The vacation domain covers the full lifecycle: acquisition (how many days are
earned), valuation (the pay and the one-third constitutional premium), the split of
the vacation across payroll competences, and the cash-basis taxation on the payment
date.

## Purpose

Compute vacation entitlement and pay: the number of days acquired (modulated by
unjustified absences during the acquisitive period), the vacation remuneration plus
the constitutional one-third, the treatment of a sold allowance where elected, and
the correct social-security, FGTS and income-tax treatment — respecting that
vacation is often **paid before** it is taken.

## Inputs

- The **acquisitive period** (start date) and the employee's absence/leave history
  during it.
- The vacation period taken (start and end), any **sold allowance** (a portion of
  the days converted to cash), and dependents for income tax.
- The salary in force and the **averages** of variable earnings (overtime, night
  premium, and others), computed by the rules appropriate to each earning type.
- The **payment date** of the vacation.

## Outputs

- Days of entitlement, vacation remuneration, the one-third premium, the allowance
  and its one-third where elected.
- Social-security and FGTS bases and amounts, and the income-tax base and
  withholding computed on the payment date.
- Where the vacation spans two months, the **per-competence split** of the days and
  premium.

## Temporal behavior

- **Acquisition** spans twelve months; unjustified absences during that window
  reduce the days acquired per the statutory table. Qualifying leaves can suspend or
  reset the acquisitive period.
- **Averages** of variable earnings follow rules that differ by earning type — some
  are updated by later raises, some are simple period averages, some are driven by
  quantity — rather than a single blanket method.
- **Payment-date taxation.** Income tax on vacation is computed on the **payment
  date** (cash basis), in a calculation separate from the monthly payroll. When the
  vacation crosses months, the days and premium are **allocated per competence** for
  accrual purposes even though the tax follows the payment date.

## Compliance dependencies

- Labor-law vacation provisions (acquisition, the one-third premium, fractioning,
  and the sale of a portion of days).
- Tax-authority guidance on the taxation of the sold allowance and its one-third.
- The [legal package](../04-compliance/legal-package.md) for the income-tax table on
  the payment date, and collective rules where they modify vacation terms.

## Auditability

Acquisition is a deterministic function of the absence history and the statutory
table. Valuation exposes the salary, the averages by earning type, the one-third,
and the allowance treatment. The per-competence split and the payment-date tax base
are explicit, so a reviewer can trace each figure.

## Validation status

`VALIDATED` (allowance sub-case `PARTIALLY_VALIDATED`). Vacation participated in the
longitudinal protocol and has a dedicated golden suite covering the acquisition
table, the averages by earning type, the competence split, and the payment-date tax
treatment. The taxation of the sold allowance's one-third follows tax-authority
guidance and is covered by tests.

## Known boundaries

- Averages are computed per the method appropriate to each earning type; exotic
  variable-earning arrangements outside the tested set are a known edge.
- The interaction of vacation with specific collective clauses is validated within
  the covered instruments; the broader collective universe is a documented
  boundary.
