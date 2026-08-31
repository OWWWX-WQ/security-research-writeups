# Authorized Security Research Portfolio

Public, sanitized evidence of independent web application security research by **@OWWWX-WQ**.

## Purpose

This repository documents security work performed only in authorized CTF, laboratory, self-owned, or explicitly approved assessment environments. It is published as verifiable evidence of practical security research experience.

All live hostnames, credentials, cookies, API keys, payment identifiers, customer data, raw exploit payloads, and confidential authorization records have been removed or generalized.

## Featured Research

### Authorized E-commerce Payment Workflow Assessment

A controlled assessment of an e-commerce payment and digital-delivery workflow, covering:

- payment callback authenticity and strict signature validation;
- merchant/provider state consistency;
- transaction identifier uniqueness and replay resistance;
- order-detail authorization and digital-goods exposure;
- HTTP/TLS, session, and cookie security;
- evidence-driven remediation and retesting.

**Read the sanitized case study:** [Authorized E-commerce Payment Workflow Assessment](reports/authorized-ecommerce-payment-assessment.md)

## Skills Demonstrated

- HTTP request and response analysis
- Business-logic and state-machine testing
- Payment workflow integrity validation
- Replay and idempotency testing
- Authentication, session, and object-access review
- Negative controls and reproducible evidence chains
- Risk classification and engineering-focused remediation
- Post-fix verification criteria

## Research Method

1. Establish written scope and testing boundaries.
2. Record a normal baseline transaction.
3. Form a falsifiable security hypothesis.
4. Test only self-created accounts, orders, and synthetic data.
5. Compare merchant state with the independent provider state.
6. Preserve redacted request/response and state-transition evidence.
7. Stop at proof of impact and avoid unnecessary data access.
8. Provide root-cause fixes and measurable retest conditions.

## Selected Results

The featured assessment identified several security-relevant design failures in a controlled environment, including:

- acceptance of an improperly validated callback authenticity value;
- merchant-side completion while the payment provider did not report success;
- reuse of one provider transaction identifier across different merchant orders;
- insufficient authorization around completed-order details;
- transport and cookie settings that increased session replay exposure.

Testing used only authorized synthetic orders. No third-party customer information or production secrets are published.

## Remediation Themes

- Verify callback signatures using canonical input, strict types, and constant-time comparison.
- Bind provider, merchant, transaction ID, order ID, currency, and amount immutably.
- Enforce a database-level uniqueness constraint for payment transactions.
- Process pending-to-paid transitions atomically and idempotently.
- Require explicit authorization for order and digital-delivery resources.
- Force HTTPS and use Secure, HttpOnly, and appropriate SameSite cookie attributes.
- Log duplicate callbacks, state mismatches, and rejected authenticity checks.

## Verification and Provenance

This repository is authored and maintained by **@OWWWX-WQ**. Original authorization records and raw test evidence are retained offline and may be made available privately to legitimate reviewers subject to confidentiality requirements.

This publication is a sanitized research portfolio, not a claim of a CVE assignment or a disclosure of an actively exploitable public target.

## Contact

For professional verification, use this GitHub profile and repository history. Additional organizational or security-team references can be provided privately where permitted.

---

Published: 31 August 2026
