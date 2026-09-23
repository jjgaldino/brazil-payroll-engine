# Worker Credit (Consignado)

Payroll-deductible worker credit is computed as a margin over **available
remuneration**, never as a flat percentage of a raw base. This distinction is the
core of the domain's correctness.

## Purpose

Compute the payroll deduction for a payroll-deductible loan under the applicable
consignment framework: determine the **available consignable remuneration**, apply
the legal margin, deduct the effective installment (never exceeding the margin, never
driving net pay negative), and defer any excess rather than silently rejecting it.

## Inputs

- The earnings subject to social-security incidence, the social-security and
  income-tax withholdings, and any compulsory (e.g. judicial) deductions — from which
  the available remuneration is derived.
- The **installment(s)** requested per contract and institution.

## Outputs

- The **available consignable remuneration** and the legal margin over it.
- The **effective deducted amount** per installment (capped at the margin), the
  **non-deducted** remainder, and a status (full / partial / not deducted for
  insufficiency).
- The corresponding payroll deduction event and the data for the eSocial
  declaration.

## Temporal behavior

- The computation is **monthly and independent**: if the installment exceeds the
  margin, the deduction is partial and the shortfall is **not** carried into the
  next month.
- Termination uses a **separate** guarantee track (a percentage of the available
  termination remuneration), not the monthly margin.

## Compliance dependencies

- The consignment law's definition of the deductible margin as a percentage of the
  **available remuneration** — computed as gross remuneration minus social security,
  income tax and compulsory deductions — not a flat percentage of a raw base.
- The worker-credit program rules for the source of the installment (an official
  query, with a manual fallback that is recorded, never silently chosen).

## Auditability

The available remuneration is decomposed **rubric by rubric** (which earnings are
included, which are excluded, and why), so the margin is fully explainable. The
engine version and legal-package version are frozen into each result. When an
official query and a manual value disagree, the engine surfaces a **review** with
both values rather than choosing silently. When two contracts contend for the same
margin, the engine flags a review rather than inventing an allocation order.

## Validation status

`VALIDATED`. The domain passed the longitudinal protocol (an employee with a credit
contract deducting within the margin) and was additionally exercised against a
**real-data probe** derived from an actual company's closing, reconciling an official
query against a manual portal value. The defining property — margin over available
remuneration, never a flat percentage of base — is a proven behavior.

## Known boundaries

- There is **no invented allocation** when multiple contracts compete for one margin;
  the engine requires a review.
- The worker-credit deduction is a monthly declaration and does not flow into the
  annual 13th-salary declaration.
- The official-source query depends on an external service; when it is unavailable,
  the manual fallback is recorded as such, not treated as authoritative.
