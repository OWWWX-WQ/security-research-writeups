# Public API Protocol Security Assessment

**Researcher:** @OWWWX-WQ  
**Assessment type:** Authorized read-only API and documentation review  
**Environment:** Public documentation plus low-frequency negative-key requests  
**Publication status:** Sanitized, target-neutral case study  
**Assessment period:** August 2026  
**CVE status:** No CVE claimed

## Executive Summary

This assessment reviewed the authentication and data-flow design of a documented HTTP API. The work combined protocol documentation review, official example-client analysis, controlled invalid-key requests, single-variable parameter mutation, duplicate-parameter testing, and error-semantics comparison.

The most important issue was an official client example that explicitly disabled TLS certificate and hostname verification. The public protocol also supported a long-lived API key in form or query parameters but did not document a request signature, timestamp, nonce, or idempotency key. Additional findings included pre-authentication action enumeration, inconsistent status/content-type behavior, and last-value-wins handling of duplicate security parameters.

No valid credential or production write operation was used. Replay impact was therefore not claimed as confirmed.

## Findings

| ID | Severity | Finding | Confidence |
|---|---|---|---|
| API-01 | Medium, conditional | API key accepted from a URL query parameter | Confirmed parsing path; valid-key exposure not exercised |
| API-02 | Medium | Action enumeration and inconsistent authentication error semantics | Confirmed |
| API-03 | Low, conditional | Duplicate action parameters use last-value-wins parsing | Confirmed prerequisite; edge disagreement not established |
| API-04 | High | Official example client disables TLS peer and hostname verification | Confirmed by code review |
| API-05 | Medium | Protocol lacks documented per-request integrity, freshness, and idempotency | Confirmed design gap |
| API-06 | Undetermined | Actual authenticated write replay impact | Not tested |

## API-01: Credentials in Query Parameters

A request containing an invalid key and a known action reached the authentication logic, while an incomplete action request followed a different route. This demonstrated that query-string credentials were parsed before rejection.

Long-lived secrets in URLs can be retained by gateways, reverse proxies, access logs, observability systems, browser history, copied links, and referrer flows. TLS does not remove server-side or intermediary URL logging.

### Remediation

- Reject credentials supplied in the URL.
- Require an Authorization header or a dedicated authentication header.
- Redact historical query-key fields from gateway, WAF, APM, and application logs.
- Use Cache-Control: no-store and a strict Referrer-Policy.

## API-02: Authentication Error Oracle

Known actions and unknown actions produced distinguishable status codes and response bodies before successful authentication. One known action returned HTTP 200 with a text/html content type even though the body represented an authentication failure, while other known actions returned 401 JSON.

This can expose the action namespace and cause SDKs, monitoring, or audit systems that rely on the HTTP status to treat an authentication failure as success.

### Remediation

- Authenticate before action dispatch, or return a uniform external error.
- Use 401 for authentication failure, 403 for authorization failure, 400 for malformed input, and 404 only for intentionally undisclosed resources.
- Return a stable JSON error schema with a request identifier.
- Require SDKs to validate both HTTP status and business error code.

## API-03: Duplicate-Parameter Semantics

Controlled requests showed that the last duplicate action value controlled the route. Reversing the order reversed the observed response class. Encoded names and values were decoded once; array-shaped action values were rejected; unknown fields were ignored.

Last-value-wins behavior is not automatically exploitable. It becomes dangerous if a CDN, WAF, signature layer, or application uses a different rule such as first-value-wins or concatenation.

### Remediation

- Reject duplicate security-relevant parameters with HTTP 400.
- Normalize input once and share the normalized representation across filtering, signature verification, and business logic.
- Include method, canonical path, caller, action, and complete body digest in the signature input.

## API-04: TLS Verification Disabled in Official Client

The published PHP example explicitly disabled both certificate-chain verification and hostname verification. An integration that copied this example would accept an untrusted certificate or the wrong hostname.

An on-path attacker could then obtain the long-lived bearer key, modify a request, or forge a response. This was classified as a client-integration vulnerability rather than evidence that the server disabled TLS verification.

### Remediation

- Remove the unsafe client options.
- Use the operating system's trusted CA store and enforce hostname verification.
- Publish corrected examples for each supported language.
- Notify integrators and require rotation of keys used by affected clients.
- Add automated tests that fail when certificate or hostname verification is disabled.

## API-05: Missing Request Integrity and Freshness

The documented protocol used a long-lived bearer key plus an action and business parameters. It did not document a signature, timestamp, nonce, idempotency key, or client transaction identifier for write operations.

This means compromise of the bearer key removes the only documented request-authentication layer, and the protocol itself provides no proof of freshness or duplicate-operation protection.

### Recommended protocol

- HMAC-SHA-256 or an equivalent authenticated request scheme.
- Signature input: method, canonical path, caller, action, canonical body digest, timestamp, and nonce.
- A short, symmetric timestamp window and atomic nonce storage.
- A caller-and-operation-scoped Idempotency-Key for write operations.
- Same key plus same payload returns the original result; same key plus different payload returns 409.

## Controlled Mutation Matrix

| Mutation | Result |
|---|---|
| Reordered key and action | Same authentication result |
| Added unknown field | Ignored |
| Single URL encoding | Parsed |
| Double encoding | Not decoded twice |
| Changed action case or added trailing space | Rejected |
| Array-shaped action | Rejected without server error |
| Duplicate action | Last value selected |

The matrix did not turn an invalid key into successful authentication, data access, or a server error.

## Testing Boundary

A placeholder nonce, timestamp, signature, and idempotency field remained at the invalid-key boundary. This did not prove that the server supported or ignored those fields. Without a dedicated credential and reversible transaction, the assessment did not send authenticated write requests and did not claim replay exploitation.

## Retest Criteria

1. Query credentials are rejected and absent from logs.
2. All invalid credentials receive one stable 401 JSON schema.
3. Duplicate critical parameters receive 400 before dispatch.
4. Corrected example clients fail closed on invalid certificates and hostnames.
5. Signatures bind all security context and reject altered fields.
6. Nonces are atomic and timestamps enforce a short window.
7. Idempotency behavior is deterministic and persisted across workers.

## Evidence Quality

Private evidence includes the documentation snapshot, official example source, response matrix, parameter mutation matrix, and an offline analyzer. No valid key, customer data, live write request, or target identifier appears in this publication.

