# Evidence Matrix (Public)

This matrix maps **public claims** to the evidence that supports them, distinguishing
what is shown publicly from what is available privately under NDA. It does not publish
sensitive internal files.

## How to read this

- **Public evidence** — a document in this repository a reader can inspect now.
- **Private evidence (NDA)** — internal reports/artifacts in the private data room,
  available under NDA. Referenced by name, not published here.

## Matrix

| Claim | Public evidence | Private evidence (NDA) |
|---|---|---|
| 24-month longitudinal validation, engine vs independent oracle, 0 final divergences | [validation-overview](../05-validation/validation-overview.md), [master-shadow-summary](../05-validation/master-shadow-summary.md) | Master Shadow close report; global reconciliation; per-domain certification artifacts |
| Persistence certified in isolated staging (reload, replay, restart, idempotency, rollback) | [persistence-validation-summary](../05-validation/persistence-validation-summary.md) | Persistence certification report; persisted chain artifact |
| Two validation findings attributed to the oracle; engine vindicated; evidence preserved | [validation-overview](../05-validation/validation-overview.md) | Detailed incident register (two incidents) with root-cause and resolution |
| Bitemporal rule resolution; retroactivity without rewriting closed payroll | [temporal-model](../02-architecture/temporal-model.md), [retroactive-payroll](../03-payroll-domains/retroactive-payroll.md) | Bitemporality design notes; retroactive/dissídio certification |
| Legal package: versioned parameters, payment-date income tax, no silent fallback | [legal-package](../04-compliance/legal-package.md) | Legal-package coverage report; temporal-basis specification |
| Collective rule package: applicability, addendum-as-delta, precedence trace | [collective-rule-package](../04-compliance/collective-rule-package.md) | Clause matrix; coverage report; instrument inventory |
| Accident-leave FGTS maintained (full base, reduced salary) | [leaves](../03-payroll-domains/leaves.md) | Leave-domain certification; golden suite |
| 13th severance deposited per rounded installment (accounting invariant) | [thirteenth-salary](../03-payroll-domains/thirteenth-salary.md) | 13th certification; incident #1 record |
| Worker credit as margin over available remuneration (never flat % of base) | [econsignado](../03-payroll-domains/econsignado.md) | Worker-credit certification; real-data probe manifest |
| eSocial: build/harden/validate + isolated signing/transport; exercised in restricted production | [esocial-architecture](../04-compliance/esocial-architecture.md) | eSocial rehearsal audit; transmission proofs; reconciliation logs |
| Immutable snapshots; append-only; reopening preserves closed snapshot | [observability](../07-security/observability.md) | Snapshot/closing design; state-machine tests |
| Judicial deductions only via versioned domain; manual bypass blocked | [security-overview](../07-security/security-overview.md) | Judicial-guard tests; SEC-00 audit |
| Tenant isolation; certificate isolation; staging/production separation | [security-overview](../07-security/security-overview.md) | SEC-00 security audit; runtime-boundary audit |

## Boundaries reflected in this matrix

- Every "validated" claim is scoped to the **internal** protocol and the **tested**
  set — not to a universal guarantee or an external certification.
- Private evidence is referenced by name only. The public repository intentionally
  does **not** contain the full technical reports, the persisted hash chain, the
  detailed incident register, real identifiers, or third-party agreement texts.
