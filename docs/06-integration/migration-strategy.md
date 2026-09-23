# Migration Strategy

Because several Brazilian payroll calculations read the past, **how much history is
migrated** determines how much can be computed without a review-required stop. Ordo
supports three strategies and is explicit about their trade-offs.

## The core tension

The engine reads history for vacation acquisition, 13th twelfths, averages,
terminations and retroactivity. Its **"trusted-since" marker** means that a
competence before the point from which a fact is trusted returns a
**review-required** state rather than calculating on unreliable data. Migration is
therefore a decision about **how far back history is trusted**.

## Strategy 1 — Full historical migration

Import the complete employment/salary/schedule/dependents history from the legacy
system, each as time segments with the trusted-since marker set at the true start.

- **Result**: history-dependent calculations run for prior competences without a
  review stop.
- **Best for**: cases needing a complete audit trail and immediate correctness on
  history-dependent domains (long-tenure vacation, mid-year 13th, terminations soon
  after cutover).
- **Cost**: the most data preparation and mapping.

## Strategy 2 — Opening snapshot

Import a single opening segment at the cutover date (trusted-since = cutover).

- **Result**: competences on/after the cutover compute normally; competences
  **before** the cutover return review-required (they cannot be recomputed).
- **Best for**: a fast start when the legacy calculation history is not trusted and
  does not need to be reproduced.
- **Cost**: history-dependent calculations that reach before the cutover need the
  relevant prior figures supplied as opening balances (e.g. accrued vacation, 13th
  progress) or are flagged.

## Strategy 3 — Hybrid

Import the last N months (or the boundaries that matter — the current vacation
acquisitive period, the current 13th year) as full segments, and use an opening
snapshot for everything earlier.

- **Result**: the calculations that reach into the recent past run; the deep past is
  a controlled review boundary.
- **Best for**: a pragmatic balance — trust the recent history, don't reconstruct the
  entire past.
- **Cost**: choosing the correct boundaries for the history-dependent domains.

## Which calculations force the decision

| Domain | Why it reaches back |
|---|---|
| Vacation | Acquisitive period (up to twelve months) and absences within it. |
| 13th salary | Qualifying months across the year. |
| Averages | Prior months of variable earnings. |
| Termination | Full history (vested vacation, tenure, contract). |
| Retroactive / supplementary | Salary history across the affected period. |

A migration that does not cover these windows will produce review-required results
for the affected employees until the needed history or opening balances are
supplied — by design, this is a **visible** boundary, not a silent miscalculation.

## Reproducing a legacy closing (optional)

Where a buyer needs the first Ordo closings to **match** the legacy system's prior
numbers exactly (including the legacy system's rounding or day-count conventions),
Ordo supports an **opt-in legacy-parity projection**: a versioned overlay that
reproduces the legacy behavior **with provenance**, without mutating the canonical
fact. The canonical result remains the Ordo-correct one; the parity projection is an
auditable, clearly-labeled compatibility layer. See
[frequency & DSR](../03-payroll-domains/frequency-dsr.md) for an example
(commercial-month divisor, decimal delay encoding).

## Honest boundaries

- Migration **connectors** for specific legacy vendors are not all built; Ordo
  provides parsers for common interchange formats and a canonical model. A specific
  connector is integration work.
- The dirty-window mechanism marks which competences must be recomputed when a fact
  is changed on a given date; it **marks**, it does not silently recompute.
