# Documentation Repository — Build Report

Build of the Ordo Payroll Core public showcase + private data room, per the
mother-instruction. **No public push was performed** (`PUBLICATION_AUTHORIZED =
FALSE`): the repository was prepared locally, sanitized, and stopped before publish.

## Parameters honored

```
DOCUMENTATION_BUILD        = APPROVED
PUBLIC_REPO_STRATEGY       = SEPARATE_REPOSITORY
SOURCE_REPO_VISIBILITY     = PRIVATE (unchanged)
PUBLIC_REPO_NAME           = ordo-payroll-core
PUBLICATION_AUTHORIZED     = FALSE  → stopped before git push
PRIVATE_REPO_HISTORY_IMPORTED = FALSE (fresh history)
```

The public repository was created as a **separate directory**
(`C:\dev\PROJETOS\ordo-payroll-core`), physically outside the private source repo,
with **no** submodule or link back to the private repo.

## Public files generated

Top-level: `README.md`, `COPYRIGHT.md`, `SECURITY.md`, `TEASER.md`, `.gitignore`,
this build report.

```
docs/01-overview/       product-overview, functional-coverage, executive-overview
docs/02-architecture/   architecture-overview, temporal-model, integration-architecture
docs/03-payroll-domains/ monthly-payroll, frequency-dsr, vacation, thirteenth-salary,
                        leaves, overtime, retroactive-payroll, termination, econsignado
docs/04-compliance/     legal-package, collective-rule-package, regulatory-watch,
                        esocial-architecture
docs/05-validation/     validation-overview, master-shadow-summary,
                        persistence-validation-summary
docs/06-integration/    canonical-payroll-history, payroll-core-contract, migration-strategy
docs/07-security/       security-overview, observability
docs/08-transaction/    acquisition-oem-overview
docs/appendices/        terminology, evidence-matrix-public
assets/diagrams/, assets/screenshots/ (placeholders; Mermaid diagrams inline in docs)
```

Total public documentation files: **29** (4 top-level + 24 docs + 1 build report),
plus 2 asset directories. Diagrams are Mermaid, embedded inline (no raster
screenshots were published — none with synthetic-only, secret-free content were
available to include).

## Private files generated (data room — gitignored, NOT published)

```
private-data-room/
  ORDO_PAYROLL_CORE_PRODUCT_TECHNOLOGY_DOSSIER.md
  VALIDATION_DOSSIER.md
  TECHNICAL_DUE_DILIGENCE.md
  IP_ASSET_INVENTORY.md
  REPOSITORY_MAP.md
  DETAILED_INCIDENT_REGISTER.md
  INTEGRATION_DOSSIER.md
  TRANSACTION_SCOPE.md
  EVIDENCE_MATRIX.md
  KNOWN_LIMITATIONS.md
```

10 private files. Excluded from git via `.gitignore` (`private-data-room/`,
`*.private.md`, `*.secret.*`, `.env*`, certificate extensions).

## Redactions performed

- **Collective instruments** described **generically** in public docs ("a sector
  agreement and a later addendum"); specific registration numbers / union names
  (SITRAEMFA/SINBFIR/SP011104/SP011135) were **not** published.
- **Real identifiers** (company CNPJs including the validation companies, the signing
  certificate holder, real receipt/protocol numbers) were **excluded everywhere**,
  including the private data room, which references them by description only.
- **Internal file paths / module names** appear **only** in the private data room,
  never in public docs.
- **Personal names**: none; validation personas are synthetic (M01–M06).

## Security / sanitization scan

Scanned the entire tree for CNPJ, CPF, eSocial receipt numbers, private-key headers,
`DATABASE_URL`/`POSTGRES_URL`, API-key/secret/bearer/password patterns, and known
real identifiers.

```
SOURCE_CODE_EXPOSED    = 0
SECRETS_EXPOSED        = 0
PERSONAL_DATA_EXPOSED  = 0
CLIENT_DATA_EXPOSED    = 0
SANITIZATION_SCAN      = PASS  (public tree clean; private room also free of raw secrets)
OPEN_SOURCE_LICENSE_ADDED = FALSE
COPYRIGHT_NOTICE       = PRESENT
```

## Claims validated (grounded in source reading)

All public claims are grounded in a read of the private source and its artifacts
(seven read-only research passes across the engine, domains, compliance, eSocial +
Rust executor, canonical history/integration, and security/observability). The
claim→evidence mapping is in `docs/appendices/evidence-matrix-public.md` (public) and
`private-data-room/EVIDENCE_MATRIX.md` (full).

## Claims requiring human review

| Item | Action |
|---|---|
| IP legal title | `TO_BE_LEGALLY_VERIFIED` — confirm authorship/assignment. |
| Security contact | `SECURITY_CONTACT_TO_BE_DEFINED`. |
| Commercial contact | `COMMERCIAL_CONTACT_TO_BE_DEFINED`. |
| Public API packaging | Marked `TARGET PUBLIC CONTRACT` where not yet a versioned public endpoint. |
| SBOM / OSS license review | DD deliverable, not produced here. |

## Legal review items

- Confirm corporate ownership and contributor arrangements before any transaction.
- Confirm no open-source license is desired (none added, per instruction).
- Review third-party name references (government platforms, norms, agreements) for
  descriptive-use appropriateness.

## Commercial review items

- Define the security, commercial and (if desired) publication contacts.
- Decide whether the public repo is published as-is or after the human review above.

## GitHub readiness

- `PUBLIC_REPO_SEPARATE_FROM_SOURCE = TRUE`
- `PRIVATE_REPO_HISTORY_IMPORTED = FALSE`
- `PUBLIC_DOCS_COMPLETE = TRUE` · `README_COMPLETE = TRUE`
- `ARCHITECTURE_OVERVIEW_COMPLETE = TRUE` · `VALIDATION_SUMMARY_COMPLETE = TRUE`
- `INTEGRATION_MODEL_COMPLETE = TRUE` · `KNOWN_BOUNDARIES_VISIBLE = TRUE`
- `SANITIZATION_SCAN = PASS`

**Ready to publish pending human authorization.** Per instruction, **no** GitHub repo
was created and **no** push was performed. To publish, a human sets
`PUBLICATION_AUTHORIZED = TRUE`, then a new public GitHub repository `ordo-payroll-core`
(visibility PUBLIC) is created and **only** the sanitized content (excluding
`private-data-room/`) is pushed. The private source repository's visibility is never
changed.
