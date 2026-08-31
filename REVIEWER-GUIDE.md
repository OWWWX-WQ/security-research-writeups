# Reviewer Guide

This page provides a short path through the portfolio for professional or security-program review.

## Three-Minute Review

1. Read the [portfolio overview](README.md) for scope and case-study links.
2. Review the [authorized payment workflow assessment](reports/authorized-ecommerce-payment-assessment.md) for direct business-impact evidence and remediation.
3. Review the [Windows desktop client study](reports/windows-desktop-client-security-review.md) for binary, runtime, and localhost trust-boundary analysis.
4. Review the [admin frontend study](reports/admin-frontend-source-exposure-and-queue-review.md) for source artifact recovery, API inventory, careful negative results, and client reliability analysis.
5. Check the [sanitized evidence ledger](EVIDENCE.md) and [provenance statement](PROVENANCE.md).

## What This Portfolio Demonstrates

- authorization-aware research planning;
- web, API, frontend, binary, and local-service analysis;
- controlled proof of impact using synthetic objects;
- independent state checks and negative controls;
- separation of direct evidence, code-level risk, conditional risk, and unverified hypotheses;
- root-cause remediation and measurable retest criteria;
- correction of an earlier conclusion when stronger evidence contradicted it.

## Strongest Direct Result

The payment workflow case documents a merchant/provider integrity mismatch: the authorized merchant test order reached a completed state while the independent provider state remained non-success. The public report excludes the live target and operational payload while preserving the test method, impact, remediation, and acceptance criteria.

## Breadth Result

The desktop-client case combines PE trust assessment, OneFile extraction, runtime process/port observation, memory reconstruction, localhost API controls, negative malware findings, and defensive architecture recommendations.

## Restraint and Accuracy

The portfolio explicitly records limits:

- public source-map exposure was not presented as an authorization bypass;
- invalid high-privilege tokens were rejected;
- authenticated API write replay was not tested;
- destructive localhost actions were not exercised;
- a mistaken UI inference was retracted after stronger evidence appeared;
- no CVE or vendor acknowledgement is claimed.

## Private Verification

Original authorization and raw evidence remain offline. Where confidentiality permits, a legitimate reviewer may request private organizational verification. The public GitHub history establishes publication and revision history but is not a substitute for third-party attestation.

