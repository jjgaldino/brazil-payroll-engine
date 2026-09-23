# Canonical Payroll History

The engine consumes a **canonical, temporal model** of the employment relationship.
An external system integrates by mapping its own data into this model — not by
matching an internal schema.

## Why a canonical model

Brazilian payroll calculations are history-dependent: vacation acquisition, 13th
twelfths, averages, terminations and retroactivity all read the *past*. A canonical
model gives the engine a consistent, temporal view of that history regardless of
which system the data came from.

## The contractual facts

The relationship is expressed as a small set of **contractual facts**, each modeled
as **time segments** with a validity period and a provenance:

- **Remuneration** — salary and salary type (monthly, hourly, daily, commission,
  mixed).
- **Role / function / occupational classification (CBO).**
- **Workweek** — weekly hours, schedule type, divisor.
- **Contract type / duration** — indeterminate, fixed-term, experience,
  intermittent, temporary, with an end date where applicable.
- **Dependents** — with **separate eligibility** for income-tax deduction and for the
  salary-family allowance.

Each segment carries `effective-from / effective-to`, an **origin** (real change,
migration baseline, import, adjustment, UI, portal), and a **"trusted-since"**
marker. For any competence, the engine resolves the segment in force on the relevant
day. If a competence falls before a fact's trusted-since point, the engine returns a
**review-required** state instead of calculating on unreliable history.

## History streams the engine consumes

Alongside the contractual facts, the engine reads history and current-period facts:

- **Employment history** (admission, and where applicable termination).
- **Salary history** (for averages and retroactivity).
- **Schedule history** (affects accruals).
- **Dependents** (by validity).
- **Frequency** (absences, delays, early departures — as sourced facts).
- **Leaves** (with their natures and effects).
- **Vacation history** (acquisitive periods and taken periods).
- **13th-salary history** (advances, provisional/definitive).
- **Termination history** (for final settlement and any post-termination
  reflections).
- **Collective context** (the applicable agreement/addendum for the worker).
- **Loans / worker credit** (installments and contracts).

## Which calculations depend on history

| Domain | History it reads |
|---|---|
| Vacation | Acquisitive period, absences during it, schedule changes. |
| 13th salary | Qualifying months across the year; leaves. |
| Averages | Prior months of variable earnings (for vacation/13th/termination). |
| Termination | The **full** employment, salary, schedule and vacation history. |
| Retroactive / supplementary | Salary history across the affected period. |

This is why migration strategy matters: the more history is available and trusted,
the more of these calculations can run without a review-required stop. See
[migration strategy](migration-strategy.md).

## Mapping from a legacy system

An external system (for example, a legacy payroll/accounting system) maps its data
into these facts and history streams via a **buyer-side adapter**. Ordo provides
parsers for common interchange formats and the canonical model; a specific vendor
connector is integration work, not a pre-shipped feature. The documentation is
deliberate about this: it describes a **target integration pattern**, and does not
claim turnkey connectors to specific vendors.

## What this document does not publish

The internal storage schema of the canonical model is not published. This document
describes the **conceptual contract** — the facts, their temporality, and the
history streams — sufficient to understand how an external system would map into it.
