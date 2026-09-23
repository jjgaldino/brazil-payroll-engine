# Payroll Core Contract

The engine's boundary is a **product-language contract**: history and facts in,
payroll result and audit trace out. This document describes that contract
conceptually.

> **Status note.** Where an operation below is not yet exposed as a public,
> documented, versioned API endpoint, treat it as a `TARGET PUBLIC CONTRACT` — the
> intended shape of the interface — rather than a shipped public API. The engine and
> its internal service boundary exist; the *public* packaging of that boundary is
> part of what an integration or OEM engagement would formalize.

## The conceptual interface

```
Canonical Payroll History
+ Current-period Facts
+ Compliance Context (competence, payment date, collective profile)
        →  Calculate
        →  Result (events, bases, charges, net) + Audit Trace + Replay Data
```

The contract speaks in **product terms** — compute a payroll, close a competence,
build an obligation event — not in database operations. Callers never issue raw
queries; the server is authoritative and performs all statutory calculation.

## Operation families

| Family | Purpose |
|---|---|
| **Facts** | Submit/resolve the contractual facts and current-period facts for an employee and competence. |
| **Calculate** | Compute the payroll (or a specific domain: vacation, 13th, termination, retroactive) for a competence. |
| **Close / reopen** | Close a competence into an immutable snapshot; reopen it while preserving the closed snapshot. |
| **Obligations** | Build, validate, and transmit the corresponding eSocial event through the isolated executor. |
| **Reconcile** | Compare computed values against an external totalization. |
| **Audit / replay** | Retrieve the execution envelope and replay data for a result. |

## Contract properties

- **Typed, explicit errors.** Failures are typed — not-authenticated, out-of-scope,
  permission-denied, validation, not-found, unavailable, conflict — and never a
  silent fallback. A missing capability is an explicit error, not a degraded result.
- **Context in the address, identity in the session.** The company/tenant context is
  part of the operation's address; the caller's identity travels as a session token,
  never as a value in a request body.
- **Server-side calculation.** Statutory computation happens on the server; clients
  consume results and never compute legal rules.
- **Idempotent obligations.** Government transmission is idempotent — a terminal event
  is never re-sent, so retries are safe.
- **Determinism.** Given the same facts and resolved rules, a calculation is
  reproducible; the audit/replay family exposes exactly what is needed to reproduce
  it.

## Result shape (conceptual)

A payroll result contains: the **events** (earnings, deductions, informational) with
their codes, references, amounts and incidences; the **derived bases** (social
security, income tax, severance fund); the **withholdings and accruals**; the
**employer charges**; the domain figures (vacation/13th/termination where relevant);
and the **audit trace** and **replay capsule** (engine version, resolved legal and
collective packages, input fingerprint) needed to explain and reproduce it.

## What is intentionally not specified here

Exact endpoint paths, request/response payload schemas, authentication scheme
details, and versioning policy are part of an integration engagement and the private
technical materials — not this public document. Publishing them prematurely would
misrepresent a `TARGET PUBLIC CONTRACT` as a shipped, stable API.
