# Authorized E-commerce Payment Workflow Assessment

**Researcher:** [@OWWWX-WQ](https://github.com/OWWWX-WQ)  
**Assessment type:** Authorized black-box web and business-logic review  
**Environment:** Owner-operated or explicitly authorized test environment  
**Publication status:** Sanitized public case study  
**Assessment period:** August 2026  
**CVE status:** No CVE claimed

## Executive Summary

This case study documents an authorized assessment of an e-commerce payment and digital-delivery workflow. The goal was to determine whether the merchant application independently verified payment authenticity, transaction ownership, amount, state transitions, and access to completed-order content.

Testing identified a critical callback-validation weakness and several related control gaps. In the controlled test environment, an invalidly typed authenticity value was accepted, the merchant order changed from pending to completed, and the independent payment-provider state did not indicate a successful payment. Additional testing showed insufficient global binding of provider transaction identifiers and inadequate protection of completed-order details.

The live hostname, commercial identifiers, account data, credentials, secrets, payment addresses, exact payloads, and raw order identifiers are intentionally omitted.

## Scope and Rules of Engagement

The assessment was limited to:

- one authorized test account;
- self-created synthetic orders;
- a designated test product;
- the merchant checkout, payment callback, order lookup, and delivery paths;
- non-destructive proof of impact;
- controlled request rates and no availability testing.

Testing stopped after a reproducible state transition or data-access proof. No unrelated customer records were intentionally accessed.

## Threat Model

The assessment considered an external attacker who can:

- create an ordinary order;
- observe their own browser and network traffic;
- submit requests to publicly reachable merchant endpoints;
- replay or modify values associated with their own transaction;
- compare merchant-visible state with provider-visible state.

The attacker was not assumed to possess server credentials, database access, payment-provider signing secrets, or administrative privileges.

## Methodology

### 1. Baseline

A normal order was created and its expected lifecycle recorded:

1. order created;
2. merchant state pending;
3. payment provider state pending;
4. valid payment confirmation;
5. merchant state completed;
6. authorized digital delivery.

### 2. Input-boundary testing

Callback parameters were tested with controlled type and value changes. Only one variable was changed per request. Negative controls were retained to distinguish a real validation weakness from a general endpoint failure.

### 3. Independent state verification

Merchant-side order status was not treated as proof of payment. The provider-side status was checked independently and compared with the merchant state.

### 4. Replay and binding tests

Provider transaction identifiers were replayed only against self-created test orders to determine whether the identifier was globally unique and immutably bound to one merchant order.

### 5. Access-control controls

Fresh browser sessions without the original order session were used as negative controls when reviewing completed-order detail access.

## Findings

| ID | Severity | Finding | Result |
|---|---|---|---|
| A-01 | Critical | Callback authenticity value was not validated with strict type and signature semantics | Confirmed |
| A-02 | High | HTTP and cookie configuration increased session replay exposure | Confirmed |
| A-03 | Medium | One provider transaction identifier could be associated with more than one merchant order | Confirmed |
| A-04 | Medium | Completed-order details relied too heavily on possession of an order identifier | Confirmed |

## A-01: Callback Authenticity Validation Failure

### Observation

Several malformed string authenticity values were rejected, but a differently typed value was accepted. The merchant endpoint returned a success response and the authorized test order changed from pending to completed.

The independent payment-provider view still showed a non-success state. This produced a reproducible integrity mismatch:

| Check | Before test | After controlled request |
|---|---|---|
| Merchant order | Pending | Completed |
| Provider payment | Pending/non-success | Pending/non-success |
| Merchant callback response | Not applicable | Success response |
| Authorized delivery state | Not released | Released or eligible for release |

### Root cause

The behavior was consistent with unsafe type coercion or a non-strict comparison in callback signature validation. Validation logic appeared to test whether two values were loosely equivalent rather than proving that a canonical message had a valid cryptographic signature of the expected type and length.

### Impact

An attacker could potentially cause unpaid orders to be treated as paid, resulting in:

- unauthorized digital delivery;
- inventory reduction;
- inaccurate accounting and reconciliation;
- duplicate downstream jobs or notification events;
- loss of trust in payment audit records.

### Remediation

- Require the signature field to be a string of an exact expected format and length.
- Canonicalize the signed fields on both producer and verifier.
- Recompute the signature only from server-trusted order data.
- Compare signatures using a constant-time strict comparison.
- Verify merchant ID, order ID, amount, currency, provider, and transaction status.
- Reject unknown fields and unexpected data types.
- Perform the pending-to-paid transition inside one atomic transaction.
- Make callback processing idempotent.

## A-02: Transport and Cookie Hardening

### Observation

The business application remained available through an HTTP entry point, and sensitive browser cookies were observed without complete transport protections. A clean-session control did not contain the authorized test order, while replaying the authorized session material reproduced access to order information.

No cookie values are included in this publication.

### Impact

An attacker able to observe unencrypted traffic could replay session or order-access material even if the cookie content itself were encrypted.

### Remediation

- Redirect all HTTP traffic to HTTPS before application processing.
- Enable HSTS after verifying complete HTTPS coverage.
- Set Secure on every session and order-access cookie.
- Set HttpOnly where client-side JavaScript access is unnecessary.
- Use an appropriate SameSite policy.
- Rotate existing session material after deployment.
- Store order authorization server-side instead of treating a replayable client cookie as the authority.

## A-03: Transaction Identifier Uniqueness

### Observation

One controlled provider transaction identifier was accepted for two different self-created merchant orders. The second order reached a completed state instead of being rejected as already bound.

### Root cause

The application checked only the current order state and lacked an enforceable global uniqueness rule for the provider transaction.

### Remediation

Create a database uniqueness constraint over the provider or merchant namespace and transaction identifier. Bind the transaction identifier to one order, amount, currency, and account. A duplicate valid callback should return a stable success response without repeating accounting, delivery, inventory, or notification side effects.

## A-04: Completed-order Detail Authorization

### Observation

A fresh session without the original order cookie or lookup secret could request a completed-order detail resource using only an order identifier. The response included the digital-delivery field or its presence indicator.

No delivery secret or customer content is included in this report.

### Remediation

- Bind order details to an authenticated owner or high-entropy access secret.
- Authorize every object request server-side.
- Avoid sequential or otherwise discoverable public identifiers.
- Separate a public payment-status page from the protected delivery resource.
- Rate-limit lookup failures and alert on enumeration behavior.
- Redact sensitive delivery data after first retrieval where business requirements permit.

## Controlled Test Matrix

| Test | Expected secure result | Observed pre-fix result |
|---|---|---|
| Invalid authenticity string | Reject; no state change | Rejected |
| Unexpected authenticity data type | Reject; no state change | Accepted; merchant order completed |
| Provider still non-success | Merchant remains pending | Merchant completed |
| Reuse transaction ID on second order | Reject and alert | Second order accepted |
| Fresh session requests completed detail | Deny protected delivery data | Delivery field exposed |
| HTTP request to business path | Redirect before cookies/data | Business response available |

## Evidence Handling

The private evidence set retained offline contains:

- timestamps and redacted request identifiers;
- request/response status and body fingerprints;
- before-and-after order-state records;
- independent provider-state observations;
- clean-session negative controls;
- test-order ownership records;
- remediation and retest notes.

This public version intentionally excludes anything that could identify or enable exploitation of a live target.

## Retest Acceptance Criteria

A remediation retest should demonstrate all of the following:

1. Unexpected callback types return a client error and never change order state.
2. A correct signature for altered amount, currency, order, or merchant data is impossible.
3. Provider non-success always leaves the merchant order pending.
4. One transaction identifier cannot bind to a second order at the database or application layer.
5. Duplicate valid callbacks are idempotent and do not repeat side effects.
6. Anonymous or unrelated sessions cannot read protected order or delivery data.
7. HTTP business requests redirect to HTTPS without exposing application content.
8. Sensitive cookies are Secure, appropriately HttpOnly, and use a defined SameSite policy.
9. Rejected callbacks and transaction mismatches produce actionable audit events.

## Research Integrity Statement

This work was performed under explicit authorization using synthetic data and self-created orders. The publication is intended to demonstrate methodology, evidence discipline, root-cause analysis, and remediation design. It does not disclose the target identity, confidential records, signing material, operational payloads, or third-party data.

Original records are retained privately for legitimate verification where disclosure is permitted.
