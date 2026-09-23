# Overtime & Night Premium

Overtime and the night premium are computed from an hour value and the applicable
rate, with their reflection into weekly paid rest (DSR) where required.

## Purpose

Value hours worked beyond the contracted schedule (overtime) and hours worked in
the night window (night premium), applying the statutory or collectively-agreed
rate, and reflect them into DSR where the rules require.

## Inputs

- The **hours** worked as overtime and, separately, the hours worked in the night
  window (a subset of worked hours).
- The **hour value**, derived from the salary and the applicable **divisor** (which
  the collective agreement may set — e.g. a 40-hour week implies a different divisor
  than the default).
- The applicable **rates** — overtime and night-premium percentages — resolved from
  the collective agreement where it overrides the national minimum.

## Outputs

- Overtime and night-premium earning events with their amounts.
- The **DSR reflection** over these variable earnings where required.

## Temporal behavior

- Rates and the divisor are resolved **by competence** from the applicable
  collective agreement; the resolution records which candidate rule was applied and
  which was rejected (for example, the national minimum superseded by a higher
  collective rate).
- The DSR reflection uses the month's actual calendar (working days vs Sundays and
  holidays).

## Compliance dependencies

- The constitutional minimum overtime premium and the statutory night-premium
  provisions.
- Collective overrides for both rates and for the divisor/workweek. In the
  instruments used in validation, for example, overtime and night-premium rates are
  set **above** the national minimums, and the engine applies the collective rate
  with a resolution trace.
- Guidance that overtime and night premium reflect into DSR.

## Auditability

The hour value, the applied rate, the resolved source (collective vs national), and
the DSR reflection are all explicit. The rate resolution carries a trace of rejected
candidates and the reason the final rate was chosen.

## Validation status

`VALIDATED` (night premium `PARTIALLY_VALIDATED`). Overtime participated in the
longitudinal protocol and is covered by a golden suite, including the collective
rate override and the DSR reflection. The night-premium override is validated within
the covered instruments.

## Known boundaries

- Overtime and night hours must be **sourced facts**; the engine does not infer
  "extra" hours from absences or schedules.
- Where a collective agreement is ambiguous about a rate and no clear legal
  relationship exists, the engine flags a review rather than assuming a rate.
