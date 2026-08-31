# Portfolio Provenance

## Status

This repository is a public, sanitized migration of records produced during authorized security exercises, self-owned system assessments, and isolated laboratory research conducted in August 2026.

The public portfolio is intended to demonstrate methodology and technical judgment. It is not presented as a vendor advisory, CVE record, employer endorsement, or independent third-party certification.

## Research Streams

| Work stream | Original record class | Public representation | Evidence status |
|---|---|---|---|
| E-commerce payment workflow | Authorized web assessment using self-created orders | Payment workflow case study | Direct state-transition and negative-control evidence retained privately |
| Windows desktop client | Authorized PE, package, runtime, and localhost API review | Desktop client architecture case study | Static artifacts, hashes, inventories, runtime metadata, and correction records retained privately |
| Public API protocol | Authorized read-only protocol and official-client review | API protocol case study | Documentation snapshot and request mutation matrix retained privately |
| Administration frontend | Authorized source-map and unauthenticated boundary review | Admin frontend case study | Artifact hashes, recovered-source inventory, and 401 boundary matrix retained privately |
| Zero-purchase CTF | Authorized exercise design using synthetic objects | CTF design case study | Challenge specification and smoke-test records retained privately |

## Date Handling

- Original research and exercise records: August 2026.
- Public sanitization and repository migration: 31 August 2026.
- Git commit dates represent public migration dates, not necessarily the first date on which a private observation was made.

## Redaction

The migration removes or generalizes:

- live target names and hostnames;
- account, customer, and organizational identifiers;
- API keys, cookies, sessions, passwords, tokens, and signing material;
- payment addresses and transaction/order identifiers;
- private IP addresses and infrastructure details;
- operational exploit payloads and protected delivery data.

## Evidence Custody

Original records remain offline in the authorized research workspace. Public reports preserve the security property, test method, result class, confidence boundary, remediation, and retest criteria without publishing data that could identify or enable exploitation of a live system.

## Corrections

When stronger evidence changes a conclusion, the new result takes precedence and the correction is recorded. A representative example in the desktop-client study documents the withdrawal of an early UI inference after pixel-level evidence contradicted it.

## Verification Limit

The Git history verifies when the public portfolio was published and changed. Verification of the underlying private engagement may require an authorized organizational contact or confidential review of original records. This repository does not manufacture that independent attestation.

