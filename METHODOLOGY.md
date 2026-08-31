# Security Assessment Methodology

This document describes the workflow used for the public case studies in this repository. It is designed for authorized web, API, and desktop-client security research.

## 1. Authorization and Scope

- Record the owner, target class, test identities, permitted actions, and stop conditions.
- Separate confirmed authorization from assumptions.
- Use self-created accounts, synthetic data, designated products, and disposable test objects.
- Exclude destructive testing, unrelated users, and availability-impacting activity unless explicitly required by the engagement.

## 2. Evidence Baseline

Before testing a security hypothesis, record normal behavior:

- request method, path, content type, and response class;
- authentication and session state;
- object ownership and expected state transitions;
- independent provider or backend state when available;
- file hashes, process identity, and environment controls for binary research.

## 3. Hypothesis-Driven Testing

Each test changes one security-relevant variable where practical. Examples include data type, parameter order, authentication state, object ownership, request freshness, or process/network state. Negative controls are kept so that a result can be distinguished from a general failure.

## 4. Confidence Labels

- **Confirmed:** reproduced with direct evidence.
- **Code-confirmed:** present in reviewed code, but production impact was not exercised.
- **Conditional:** a necessary weakness was confirmed, while an additional prerequisite remains unverified.
- **Not established:** testing did not support the claim.
- **Retracted:** later evidence invalidated an earlier interpretation.

These labels prevent source-code observations, design concerns, and demonstrated exploit impact from being presented as equivalent.

## 5. Evidence Handling

- Preserve timestamps, hashes, state transitions, and redacted request/response fingerprints.
- Do not publish credentials, cookies, API keys, payment identifiers, customer data, private hostnames, or operational payloads.
- Keep raw authorization and evidence offline when public disclosure is not permitted.
- Record corrections when later evidence changes a conclusion.

## 6. Severity

Severity considers exploit prerequisites, affected trust boundary, confidentiality/integrity/availability impact, business consequences, and reliability. A public source map is not automatically an authorization bypass; a client-side bug is not automatically a server compromise; and a suspected weakness is not reported as confirmed without evidence.

## 7. Remediation and Retest

Every material finding should include:

1. root cause;
2. engineering remediation;
3. negative tests;
4. measurable acceptance criteria;
5. regression and monitoring guidance.

## 8. Publication Standard

Public case studies are target-neutral. They describe the security property, methodology, evidence quality, result, and remediation without identifying or enabling exploitation of a live system.

