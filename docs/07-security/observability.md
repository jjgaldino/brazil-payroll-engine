# Observability

Brazil Payroll & Compliance Engine is built to be **explained and reproduced**. Every result carries
the data needed to understand it; every closed competence is an immutable, hashed
snapshot; and the validation chain can be replayed from storage.

## Execution envelope

Each calculation runs within an **execution envelope** that records the context that
produced the result: the competence and payment date, the resolved legal package, the
resolved collective package, the known-rules-as-of date, and the payment dates. The
envelope is what lets a reviewer see *under which rules* a number was computed, not
just the number.

## Replay capsule

A **replay capsule** is the minimal evidence needed to reproduce and analyze a result
or a divergence: the engine version, the legal package, an input fingerprint, and the
field-level values. It contains no personal data, so it can be used for offline audit
and incident analysis without handling sensitive information.

## Calculation trace

Tax computations expose their internal structure — the social-security base, ceiling
and per-bracket breakdown; the income-tax base, deductions, applied bracket and any
reduction; and which earnings entered each base. Each payslip event records exactly
which rubric produced which amount under which incidence. The trace is deterministic:
the same input yields the same trace.

## Immutable snapshots

A closed competence produces a **versioned snapshot** that captures the facts used,
the resolved rules (including the collective-rule snapshot and legal-package version),
and the engine version. Snapshots are **append-only**: reopening a competence creates
a new cycle while **preserving** the prior snapshot; the historical version is never
deleted. Any attempt to mutate a closed snapshot is rejected as an invariant failure.
A closing follows an explicit state machine (open → calculated → reviewed →
ready-to-close → closed → reopened), and the corresponding obligation event is emitted
as a **consequence** of closing, not a cause.

## Incident classification

A divergence against an external system is captured as a **classifiable incident**:
each field difference is compared with a tolerance and assigned a category (rounding,
data input, temporal, collective parameter, engine, external system, declaratory, or
unknown). The engine never invents a classification — a difference above tolerance is
`unknown` until a human classifies it using the replay capsule. This keeps the
comparator a **reporter**, never a silent "corrector".

## Snapshot hashes and the validation chain

The validation chain hashes each competence's snapshot with a canonical, order-
independent hash, and links each node to the prior one (previous-hash equals the prior
snapshot-hash). This makes the chain **tamper-evident** and **replayable**:

- **Hash-chain integrity** — the chain is verified from the genesis node to the last.
- **Replay from storage** — each competence is recomputed from its persisted
  previous-hash and must reproduce the stored snapshot-hash.
- **Process-restart replay** — the chain was reconstructed and replayed in a new
  process reading only from the database.

See [persistence validation summary](../05-validation/persistence-validation-summary.md).

## Regulatory trace

Rule resolution is traceable end to end: the legal package names the resolved version
and its source; the collective package records the precedence from national baseline
to final rule with rejected candidates and reasons; the regulatory-watch model tracks
how a change signal moved from captured to applied. Together these let a reviewer
answer "why this rule, and since when?".

## Governance guard

During validation, a **stop guard** enforces that when a real discrepancy is raised,
no mutation, re-run-with-fix, or advance can occur until a human resolution is
recorded — only read, diagnose, replay and evidence-collection are permitted. This
prevents a validation stop from being silently "fixed away" and is why the two
findings during Master Shadow were attributed and resolved transparently.
