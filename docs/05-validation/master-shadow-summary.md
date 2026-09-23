# Master Shadow — Public Summary

"Master Shadow" is the engine's internal longitudinal validation protocol. It runs the
production engine against an independent oracle across a two-year horizon and freezes
the outcome as an immutable, replayable chain.

## Aggregate figures

```
24 consecutive payroll competences (2024-09 → 2026-08)
6 synthetic personas
10 payroll domains
185 engine-vs-independent-oracle validations
0 final divergences
deterministic replay
immutable snapshot chain
```

## What it is

- A **synthetic** company and workforce (six personas, each exercising a different
  axis: a control baseline, frequency, salary-substitution/worker-credit, leaves,
  vacation/averages/13th, and termination), seeded deterministically.
- A **month-by-month** run over 24 competences, each producing an immutable snapshot
  linked into a hash-chain.
- An **independent oracle** that does not import the engine's calculation code, so
  agreement is meaningful.
- **No external effects**: no eSocial, severance-fund, tax-authority, email or
  webhook side effects during the calculation certification.

## Domains covered

Monthly payroll (salary → social security / income tax / severance fund /
salary-family), retroactive/supplementary payroll (two collective adjustments), 13th
salary, vacation with averages, frequency with DSR, leaves including accident-leave
severance-fund maintenance, overtime with DSR, worker credit with salary
substitution, collective contributions (via an auxiliary probe covering the
applicable / opposition / exempt / missing-fact states), and termination — each with
zero engine-vs-oracle divergence.

## Findings, handled openly

Two discrepancies were found; both were in the **oracle**, and the engine was
vindicated in each:

1. A 13th-salary severance-rounding difference (three cents) — the engine's
   per-installment rounding was correct.
2. A vacation income-tax temporal-resolution difference — the engine's payment-date
   (cash-basis) resolution was correct.

Neither production code nor any prior snapshot was mutated to accommodate a finding.
A mutation guard was added after the first incident to require a recorded human
resolution before any change during a validation stop.

## Honest framing

- This is an **internal** validation protocol, **not** a third-party certification.
- The calculation validation ran deterministically in-process; the persistence
  layer was validated separately (see
  [persistence validation summary](persistence-validation-summary.md)).
- Human-facing evidence documents (payslips, payroll registers, a termination term)
  were derived from the certified data for illustration; they are outputs of the
  product's document generators, not part of the oracle.

## Conclusion

Across 24 months and 10 calculation domains, the production engine and an independent
oracle agreed to the cent, with deterministic replay and an intact snapshot chain.
Within the tested scope, Brazil Payroll & Compliance Engine behaved as a **longitudinally consistent
payroll operation**, not merely a correct one-shot calculator.
