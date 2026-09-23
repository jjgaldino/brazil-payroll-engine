# Frequency & DSR

Frequency is modeled as a **domain of temporal facts**, not as ad-hoc deductions.
Absences, delays and early departures are recorded as facts with dates and
durations; the engine values them and derives the weekly paid-rest (DSR) impact.

## Purpose

Represent attendance events (absence, delay, early departure) as sourced facts,
value them correctly, reduce the remuneration base **semantically** where the law
requires, and derive the loss of weekly paid rest (DSR) when an unjustified absence
forfeits it.

## Inputs

- **Frequency occurrences** — each with a type (absence / delay / early departure),
  an occurrence date, the competence it applies to, an optional duration (for
  delays/early departures), and a justification flag.
- The monthly salary and the days in the month (for valuation).
- A **versioned divisor rule** (calendar-days vs a legacy commercial-month
  convention) — the canonical behavior uses real days; legacy conventions are
  opt-in for compatibility only.

## Outputs

- Frequency events (absence, delay/early-departure, and DSR forfeiture) with their
  amounts and a marker indicating they **reduce the base** (as opposed to being a
  net-pay deduction).
- A trace linking each derived event back to the originating occurrence (date,
  week, type).

## Temporal behavior

- An occurrence's **date can predate** the competence it is applied to; both dates
  are carried, so a late-arriving absence can affect the correct payroll month.
- DSR forfeiture is computed **per ISO week**: one weekly rest is lost when a week
  contains at least one unjustified absence, following the statutory rule.
- Delays and early departures are valued from the salary and contracted hours; the
  encoding of duration (minutes vs a legacy decimal convention) is versioned.

## Compliance dependencies

- Labor-law provisions on absences and weekly paid rest.
- Collective overrides for the divisor and for justified-absence rules, where the
  applicable agreement provides them.

## Auditability

The base-reduction marker is **explicit**: only genuine frequency events reduce the
social-security/income-tax/FGTS bases. Ordinary deductions (transport voucher,
consigned credit) do **not** reduce those bases — a distinction the engine makes
deliberately, because an absence reduces the remuneration *due*, whereas a
deduction is taken *from* net pay. Every derived event is traceable to its source
occurrence.

## Validation status

`VALIDATED`. Frequency + DSR participated in the longitudinal protocol and has a
dedicated golden suite covering: multiple absences in the same week (one DSR lost),
absences across different weeks (multiple DSR lost), justified-absence exemption,
and delay valuation. A real-data probe confirmed correct delay valuation from the
salary/hours base rather than a legacy flat convention.

## Known boundaries

- **No inference.** The engine never presumes "no record ⇒ absence"; frequency
  facts must be sourced. If a justification is unknown, the DSR impact is flagged
  as an input gap rather than assumed.
- **Legacy conventions are opt-in.** Commercial-month divisors and decimal delay
  encodings exist only as an explicit compatibility projection for reproducing a
  prior system's closing; they never alter the canonical result. See
  [migration strategy](../06-integration/migration-strategy.md).
