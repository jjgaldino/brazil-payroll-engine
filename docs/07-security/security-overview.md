# Security Overview

*High-level posture only. Exploitable detail, threat models, and the internal
security audit are part of the private, NDA-gated materials.*

Ordo Payroll Core is designed so that trust boundaries are enforced in the
**authoritative server**, secrets are isolated, and the record of what happened is
**immutable and auditable**.

## Tenant and workspace isolation

The system is multi-tenant by design (an accounting office holds companies; companies
hold employees and payrolls). The client UI is **not** a security boundary:
authorization is enforced at the service boundary, where the caller's scope is
derived from the server-side session and validated against the object being accessed.
Operations verify that the target company is within the caller's scope before acting.

## Access control

Authorization follows a permission model combined with domain-state checks
(attribute-based). A permission is required to perform an operation, **and** the
domain state must allow it — for example, recalculating a **closed** payroll requires
the reopen permission, not merely the calculate permission. Enforcement is at the
business-logic entry point, so a hidden button in a UI is never equivalent to
authorization.

## Staging / production separation

Validation and experimentation run in an **isolated staging** environment on
namespaced, synthetic datasets. The longitudinal validation and the persistence
validation were performed there, without touching production and without
transmitting any external obligation.

## Certificate isolation

The eSocial signing certificate is held in an encrypted **vault** (authenticated
encryption; the master key is provided externally and never stored with the data or
in code). Only non-sensitive metadata is ever exposed to clients or API responses.
The certificate material is resolved **only at runtime**, inside the isolated
signing/transport executor, and is never persisted in a job record, returned to a
client, or written to a log. Logs redact secret-bearing fields. See
[eSocial architecture](../04-compliance/esocial-architecture.md).

## Data integrity

- **Immutable closings.** A closed competence is a versioned, frozen snapshot;
  reopening never destroys the prior snapshot (see [observability](observability.md)).
- **Judicial-deduction integrity.** Alimony/garnishment and similar judicial amounts
  can only come from a **versioned judicial domain**; they are rejected if injected
  through the manual event channel — a guard enforced at the point where the payslip
  is produced.
- **No silent fallback.** Missing legal or collective coverage stops the calculation
  with an explicit error rather than producing a wrong-but-plausible number.
- **Parameterized data access** and the absence of dynamic-evaluation patterns in the
  calculation and service layers.

## Internal security review

An internal security review ("SEC-00") covered cross-tenant access, UI-versus-service
authorization, credential/key handling (including password hashing), sensitive-data
redaction in logs, and duplicate-transmission prevention. The findings and their
remediation are part of the private technical due-diligence materials, shared under
NDA, and are not published here.

## Honest boundaries

- The security posture described here is the current design; certain properties are
  strengthened as the deployment model evolves (single-tenant desktop versus
  multi-tenant cloud), and those evolutions are certified separately.
- Deep security architecture and threat modeling are **not** published in this public
  repository, by policy.
