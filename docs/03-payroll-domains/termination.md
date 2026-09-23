# Termination

Termination consumes the employee's full history to compute the final settlement,
freezes it as an immutable snapshot, and feeds the corresponding eSocial event.

## Purpose

Compute the settlement due on termination of employment: the salary balance, the
proportional 13th salary, vacation (vested and proportional, with the one-third),
notice-period effects, and the severance-fund treatment (including the penalty where
due), applying the rights matrix appropriate to the **cause** of termination.

## Inputs

- The **cause** of termination (e.g. dismissal without cause, resignation, dismissal
  for cause, mutual agreement, end of fixed term).
- Admission and termination dates, the salary and averages, the vested-vacation
  periods, the notice type, dependents for income tax, and the severance-fund
  balance where relevant.

## Outputs

- The settlement **verbs**: salary balance, indemnified notice (where applicable),
  proportional 13th, vested and proportional vacation plus the one-third, and the
  severance-fund penalty where due.
- The **deductions**: social security and income tax on the appropriate bases.
- The **gross, deductions and net** of the settlement, and an immutable snapshot.

## Temporal behavior

- The calculation is **frozen at the termination date** as a deterministic snapshot;
  it is not recomputed. A post-transmission correction is handled through an eSocial
  rectification, not a "complementary" recomputation.
- **Indemnified notice** projects forward, adding the corresponding twelfths of 13th
  and vacation for the projected period.
- If a collective raise applies retroactively to a period covered by the
  termination, the retroactive difference is handled by the
  [retroactive/supplementary](retroactive-payroll.md) domain, not by rewriting the
  termination.

## Compliance dependencies

- The rights matrix by cause of termination (which verbs are due, the notice
  treatment, and the severance-fund penalty percentage).
- The statutory rules on the settlement deadline and on fixed-term contracts
  (early-termination indemnities and assecuratory clauses).
- Guidance that proportional vacation is due except in dismissal-for-cause.

## Auditability

The rights matrix makes the entitlements explicit by cause. The incidence matrix
makes clear which verbs enter which base (for example, indemnified notice enters the
severance-fund base but not social security or income tax; indemnified vacation
enters none). The snapshot is hashed deterministically, and the termination's
competence binds it to the payroll.

## Validation status

`VALIDATED`. Termination passed the longitudinal protocol (a resignation case
consuming the full history) with the settlement matching the independent oracle to
the cent, and is covered by a dedicated golden suite. Two historical valuation
findings (twelfths of 13th and proportional-vacation twelfths) were resolved in the
engine's favor.

## Known boundaries

- Apprentice terminations follow a separate statutory path, not the general matrix.
- Certain special fixed-term regimes are explicitly gated pending their own resolver
  rather than approximated.
- Multiple-employment terminations are computed independently per contract.
