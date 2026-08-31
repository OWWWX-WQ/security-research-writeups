# Admin Frontend Source Exposure and Queue Review

**Researcher:** @OWWWX-WQ  
**Assessment type:** Authorized frontend artifact and unauthenticated boundary review  
**Environment:** Production-style React administration frontend  
**Publication status:** Sanitized, target-neutral case study  
**Assessment period:** August 2026  
**CVE status:** No CVE claimed

## Executive Summary

This assessment analyzed a React administration frontend whose production bundles referenced publicly downloadable source maps. Offline recovery produced complete original source content, a large API definition inventory, route metadata, authentication and refresh logic, high-privilege workflow definitions, and asynchronous task-polling behavior.

The source-map exposure materially reduced the cost of mapping the administrative attack surface, but it did not itself bypass authorization. Low-frequency controls against eight sensitive API paths all returned 401, and invalid high-privilege login tokens were rejected. Two reliability defects were confirmed in code: unbounded recursive task polling and an unsettled request queue when token refresh failed.

No administrator credential, private token, production write operation, or task creation was used.

## Findings

| ID | Severity | Finding | Confidence |
|---|---|---|---|
| ADM-01 | Medium | Public production source maps expose original administrative source and workflows | Confirmed |
| ADM-02 | Medium, conditional | High-privilege bearer/magic-link token is carried in the URL path | Design confirmed; takeover not established |
| ADM-03 | Low | Background task checks use unbounded recursive polling | Code-confirmed |
| ADM-04 | Low | Token-refresh failure leaves retry promises unsettled | Code-confirmed |

## ADM-01: Production Source-Map Exposure

Two production JavaScript bundles contained explicit source-map references, and all four artifacts were anonymously retrievable. Offline parsing recovered:

- 1,241 original source entries with embedded source content;
- 448 API call definitions;
- 75 frontend routes;
- 59 high-sensitivity API candidates;
- authentication, refresh, high-privilege login, provider, payment, order, configuration, and task-status workflows.

No high-value hardcoded private key, cloud secret, payment secret, or bearer token was found in the reviewed source. The exposure nevertheless gave an unauthenticated observer precise endpoint, parameter, state-machine, and workflow knowledge.

### Remediation

- Disable source-map publication in production builds, or upload maps only to an access-controlled error-monitoring service.
- Deny map-file requests at both origin and CDN layers.
- Purge historical map objects; removing the HTML reference is insufficient.
- Scan build artifacts for credentials and rotate any secret ever embedded in source.
- Continue to enforce server-side authentication, function authorization, and object authorization on every administrative API.

## ADM-02: High-Privilege Token in URL Path

Recovered code showed a public frontend route that read a high-privilege token from a URL path, exchanged it through a POST authentication request, and accepted the resulting access token only when a specific privilege claim was present.

Boundary tests showed a minimum token length and rejection of invalid or expired values. Without a legitimate token, the assessment could not test entropy, time to live, single-use behavior, account binding, or replay. No account takeover was claimed.

Path-based bearer tokens can enter browser history, server request targets, reverse-proxy logs, WAF records, observability systems, and screenshots.

### Remediation

- Generate at least 128 bits of cryptographic entropy and store only a server-side hash.
- Bind the token to administrator, purpose, audience, tenant, and intended session.
- Use a minutes-long lifetime and atomic single consumption.
- Avoid URL path/query transport. A frontend may receive a fragment and exchange it through a POST body because fragments are not sent in the HTTP request.
- Apply Referrer-Policy: no-referrer, log redaction, rate limits, and anomaly alerts.

## ADM-03: Unbounded Background Polling

Two independent frontend workflows started a server-side task and recursively polled its status at a fixed one-second interval. The code had no maximum retry count, total deadline, exponential backoff, jitter, component-unmount cancellation, or termination for unknown states.

Production load impact was not exercised because creating a real administrative task was outside the test boundary.

### Remediation

- Add a total deadline and maximum attempt count.
- Use exponential backoff with jitter and honor Retry-After.
- Cancel on navigation or component unmount through AbortController.
- Terminate and report unknown states.
- Bind task identifiers to the initiating user and tenant.

## ADM-04: Unsettled Refresh Queue

The request layer kept a global queue while one access-token refresh was in progress. On success, queued operations were replayed and the queue was cleared. On failure, the application logged out but did not reject or clear existing queued promises and could enqueue the current failed request again.

This is a client availability and resource-lifecycle defect, not a server-side authorization bypass.

### Remediation

- Capture the refresh error once.
- Reject every queued promise with the same normalized error.
- Clear the queue in a finally block.
- Reject the current request rather than re-enqueueing it.
- Add timeout and cancellation to refresh and replay operations.

## Unauthenticated Boundary Controls

Eight known sensitive administrative paths were requested once from a fresh unauthenticated session. They included authentication configuration, order listing, provider settings, task status, token retrieval, and payment logging. All returned 401 with a stable JSON credential error.

Additional controls showed:

- the high-privilege login endpoint accepted only POST;
- an empty refresh request without a cookie returned 401;
- a short high-privilege token was rejected for length;
- a correctly sized invalid token was rejected as invalid or expired;
- OPTIONS did not bypass authentication on a protected order path.

These negative results are included because public source exposure and endpoint recovery must not be misreported as an authentication bypass.

## Retest Criteria

1. Source-map artifacts are unavailable at origin and CDN, including historical names.
2. Build pipelines fail when production maps or private credentials are present.
3. High-privilege links are short-lived, single-use, scoped, and absent from server-visible URLs.
4. Pollers terminate on deadline, unknown state, navigation, and cancellation.
5. Refresh failure rejects and clears every queued operation.
6. Administrative APIs continue to return 401/403 independently of frontend route state.

## Evidence Quality

Private evidence includes bundle and map hashes, recovered source inventories, endpoint and route catalogs, secret-scan results, exact code locations, and the unauthenticated request matrix. The public version omits bundle names, API paths, target identity, and token material.

