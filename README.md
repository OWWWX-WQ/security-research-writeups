# Authorized Security Research Portfolio

Public, sanitized evidence of independent web, API, and desktop-client security research by **[@OWWWX-WQ](https://github.com/OWWWX-WQ)**.

## Purpose

This repository documents work performed only in authorized CTF, laboratory, self-owned, or explicitly approved assessment environments. Live hostnames, credentials, cookies, API keys, payment identifiers, customer data, private infrastructure details, and operational exploit payloads are removed or generalized.

**For reviewers:** start with the [Reviewer Guide](REVIEWER-GUIDE.md), then check [Provenance](PROVENANCE.md) and the [Sanitized Evidence Ledger](EVIDENCE.md).

## Case Studies

| Case study | Discipline | Evidence focus |
|---|---|---|
| [Authorized E-commerce Payment Workflow Assessment](reports/authorized-ecommerce-payment-assessment.md) | Web / business logic | Callback authenticity, state consistency, transaction binding, order authorization |
| [Windows Desktop Client Security Architecture Review](reports/windows-desktop-client-security-review.md) | PE / runtime / local API | Protected PE triage, OneFile extraction, process mapping, localhost authorization |
| [Public API Protocol Security Assessment](reports/public-api-protocol-security-assessment.md) | API / protocol | Query-key handling, error semantics, parameter pollution, TLS examples, replay design |
| [Admin Frontend Source Exposure and Queue Review](reports/admin-frontend-source-exposure-and-queue-review.md) | Frontend / API / reliability | Source maps, endpoint inventory, privileged links, polling and refresh queues |
| [Zero-Purchase Business-Logic CTF Challenge Design](reports/ctf-zero-purchase-challenge-design.md) | CTF / exercise design | Authorized scope, synthetic data, safety controls, judging and smoke tests |

## Capabilities Demonstrated

- HTTP request and response analysis
- Web and API attack-surface mapping
- Authentication, session, and object-access review
- Business-logic and state-machine testing
- Payment integrity, replay, and idempotency testing
- Frontend source-map and asynchronous workflow analysis
- PE triage, protected-binary analysis, container extraction, and runtime observation
- Negative controls, evidence fingerprints, correction records, and retesting
- Root-cause remediation and defensive architecture design

## Research Standard

1. Establish explicit authorization, test identities, scope, and stop conditions.
2. Record a normal baseline before testing a hypothesis.
3. Change one security-relevant variable where practical.
4. Use synthetic accounts, objects, orders, and data.
5. Separate confirmed, code-confirmed, conditional, negative, and retracted results.
6. Preserve reproducible evidence without publishing sensitive material.
7. Stop at proof of impact and provide measurable remediation criteria.

Read the full [Security Assessment Methodology](METHODOLOGY.md) and [Responsible Research and Disclosure Policy](DISCLOSURE.md).

## Verification and Provenance

This portfolio is authored and maintained by **@OWWWX-WQ**. Original authorization records and raw evidence are retained offline and may be made available privately to legitimate reviewers where confidentiality requirements permit.

The repository does not claim a CVE assignment, vendor acknowledgement, employment relationship, or production impact that has not been independently established.

---

Last updated: 31 August 2026

