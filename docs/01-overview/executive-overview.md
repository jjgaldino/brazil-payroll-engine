# Executive Overview

*For non-technical and semi-technical evaluators (founders, corporate
development, M&A, product leadership). One page of substance.*

## What is being offered

**Brazil Payroll & Compliance Engine** — the calculation-and-compliance engine that
originated inside Ordo (a Brazilian payroll product) and is offered as an
**independent technology asset**. It can be evaluated and integrated **as an engine**,
independently of any end-user application.

## The thesis

Brazilian payroll correctness is a **temporal rule-resolution problem**. The value
is not "computing a payslip"; it is deciding, consistently and auditably, *which
rule applies to which worker in which month, known as of when, and paid on which
date* — and keeping closed periods trustworthy while handling retroactive changes.
Brazil Payroll & Compliance Engine encodes that as an explicit, versioned, auditable system and has
validated it **longitudinally**, not just on isolated cases.

## What makes it defensible

- **Bitemporal rule engine.** Effective-time and knowledge-time are separate axes.
  A collective raise agreed today but effective months ago is paid as a
  supplementary run **without rewriting closed payrolls**. This is the hard part
  of Brazilian payroll, and it is a genuine differentiator.
- **No silent fallback.** Missing legal/collective coverage raises an explicit
  error instead of quietly applying a wrong rule — the failure mode that creates
  latent tax/labor liabilities in most systems.
- **Longitudinal validation.** The production engine was checked against an
  **independent oracle** across 24 consecutive months and 10 domains, with **zero
  final divergences**. Two issues found were in the *oracle*; the engine was
  vindicated, and this is documented openly.
- **Immutable, auditable output.** Closed competences are versioned and frozen;
  every result can be explained and replayed.

## Validation snapshot

```
24 consecutive competences   ·   6 synthetic personas   ·   10 payroll domains
185 engine-vs-oracle checks  ·   0 final divergences     ·   deterministic replay
24/24 persisted snapshots (isolated staging) with DB reload + process-restart replay
```

Internal protocol ("Master Shadow"). Not a third-party certification.

## What a buyer gets

- A payroll/compliance engine covering the core Brazilian domains (monthly,
  vacation, 13th, termination, leaves, frequency/DSR, overtime,
  retroactive/supplementary, worker credit) plus eSocial event building and an
  isolated signing/transport executor.
- A **canonical integration model**: feed history + facts, receive an auditable
  result — usable by an ERP/HCM/HRTech without adopting the Ordo product.
- A validation corpus and audit machinery (snapshots, replay, incident
  classification) suitable for regulated environments.

## Honest boundaries

The validated scope is finite: not every collective agreement is implemented, not
every employment category is longitudinally validated, migration connectors are
not all built, and no external certification has been performed. These limits are
stated explicitly throughout the documentation.

## Possible structures

Technology/IP acquisition · exclusive license · non-exclusive OEM · white-label ·
hosted payroll API · dedicated deployment. Pricing and definitive legal terms are
handled privately.

## Next step

A private data room supports technical, legal and commercial due diligence under
NDA. Contact [contato@inventaresolutions.com.br](mailto:contato@inventaresolutions.com.br).
