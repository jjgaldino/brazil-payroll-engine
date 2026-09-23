# Leaves

Leaves (medical, accident, maternity, military service, paid/unpaid statutory
leaves) are modeled as a **taxonomy of natures and effects**, then reflected into
payroll, vacation, 13th salary and the severance fund.

## Purpose

Classify a leave by its legal nature and derive its effects: how the days split
between employer-paid and social-security-paid, whether the acquisitive period for
vacation is affected, whether the 13th salary is reduced, and — critically —
whether the severance fund (FGTS) is **maintained** during the leave.

## Inputs

- The **leave type** (e.g. common illness, work accident, maternity, military
  service, paid/unpaid statutory leave) and its period (start, end or expected/actual
  return).
- The salary in force and the days in the month.

## Outputs

- The split of days into **employer-paid** and **social-security-paid**.
- Reflections into vacation (acquisitive-period effect) and 13th salary (whether
  the period reduces the bonus).
- The **FGTS treatment** during the leave, including the base on which it is
  maintained where the law requires it.

## Temporal behavior

- The employer-paid vs social-security-paid split depends on the leave's nature and
  duration (for example, the first days of an illness are employer-paid; the
  remainder shifts to social security).
- **Acquisitive-period effects**: extended benefits or long paid leaves can
  suspend or reset the vacation acquisitive period.
- **Long leaves and collective raises**: when the FGTS is maintained during a long
  leave, the base reflects the salary **in force in that competence**, so a raise
  applied during the leave is respected.

## Compliance dependencies

- The severance-fund law's provision that the fund is maintained during accident,
  maternity and military-service leaves.
- The social-security provisions on benefit responsibility and the day split.
- Guidance that accident-related absence does not reduce the 13th salary within the
  period, and the acquisitive-period rules for vacation.

## Auditability

Leave classification uses a **frozen matrix** of natures and effects, not heuristic
inference. The employer/social-security day split, the vacation and 13th reflections,
and the FGTS base are each explicit. The **accident-leave FGTS-maintained** case —
full base with reduced salary — is a deliberately preserved proof.

## Validation status

`VALIDATED`. The leave domain passed the longitudinal protocol (including the
accident-leave FGTS-maintained case) and is covered by a dedicated golden suite.
Maternity payment-responsibility and contribution rules are `IMPLEMENTED` and
versioned.

## Known boundaries

- Open-ended leaves (no end date) are handled conservatively: duration-dependent
  effects apply only when a return date exists.
- Leave-type mapping depends on correctly sourced facts; an ambiguous or
  mis-encoded type surfaces as an input gap rather than a silent classification.
