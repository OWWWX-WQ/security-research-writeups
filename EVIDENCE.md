# Sanitized Evidence Ledger

This ledger explains the evidence behind each public case study without exposing target-identifying or operationally sensitive material.

## Evidence Classes

- **Direct:** observed request, response, file, process, state transition, or independent provider result.
- **Code-confirmed:** present in reviewed source or binary artifacts; production impact not necessarily exercised.
- **Negative control:** expected denial, unchanged state, clean session, invalid fixture, or absence of a searched behavior.
- **Exercise artifact:** challenge design, synthetic fixture, or organizer smoke test rather than a live-system finding.

## Payment Workflow

| Evidence ID | Class | Sanitized observation |
|---|---|---|
| PAY-01 | Direct | Authorized merchant order changed from pending to completed after an invalidly typed authenticity value was accepted |
| PAY-02 | Direct, independent | Payment-provider view remained non-success while the merchant order was completed |
| PAY-03 | Direct | One controlled provider transaction identifier was accepted for two self-created merchant orders |
| PAY-04 | Negative control | Fresh session lacked the original order context before the protected-detail check |
| PAY-05 | Direct | Completed-order detail returned a protected delivery field or presence indicator based only on the order identifier |

Private records retain timestamps, redacted request fingerprints, state snapshots, and test-order ownership. No payment secret or delivery content is public.

## Windows Desktop Client

| Evidence ID | Class | Sanitized observation |
|---|---|---|
| WDC-E01 | Direct artifact | Primary native executable was unsigned and strongly virtualized with high-entropy executable sections |
| WDC-E02 | Direct artifact | OneFile container was parsed to completion and produced an inner executable plus more than one thousand runtime/dependency entries |
| WDC-E03 | Negative control | Inventory did not reveal a separate script launcher or second executable payload beyond the packaged inner application |
| WDC-E04 | Direct runtime | Thirteen loopback HTTP listeners were associated with the companion service tree |
| WDC-E05 | Direct runtime | Low-impact debug and invalid-fixture requests reached handlers without a 401/403 authentication boundary |
| WDC-E06 | Negative control | No valid destructive parameter was sent to credential, administrator, restart, or session-export operations |
| WDC-E07 | Correction | An early core-UI interpretation was withdrawn after pixel evidence showed an account/password gate |

Private records retain sample hashes, extraction inventories, process/port timelines, runtime-memory verification, and response fingerprints.

## Public API Protocol

| Evidence ID | Class | Sanitized observation |
|---|---|---|
| API-E01 | Direct | Invalid query-string key plus known action reached API authentication logic |
| API-E02 | Direct | Known and unknown actions produced distinguishable pre-authentication status/body classes |
| API-E03 | Direct | Reversing duplicate action order reversed the result, confirming last-value-wins application parsing |
| API-E04 | Code-confirmed | Official example client disabled certificate-chain and hostname verification |
| API-E05 | Documentation | Published write protocol did not specify request signature, timestamp, nonce, or idempotency key |
| API-E06 | Negative boundary | No valid key or production write request was used; authenticated replay impact was not claimed |

## Administration Frontend

| Evidence ID | Class | Sanitized observation |
|---|---|---|
| ADM-E01 | Direct artifact | Public production source maps contained 1,241 embedded source entries and enabled recovery of 448 API call definitions |
| ADM-E02 | Code-confirmed | High-privilege login token was read from a URL path before server exchange |
| ADM-E03 | Code-confirmed | Two task workflows recursively polled at a fixed interval without deadline, backoff, or cancellation |
| ADM-E04 | Code-confirmed | Refresh failure did not reject and clear queued request promises |
| ADM-E05 | Negative control | Eight sampled sensitive administrative paths returned 401 from a fresh unauthenticated session |
| ADM-E06 | Negative control | Invalid high-privilege tokens were rejected; takeover was not claimed |

Private records retain bundle/map hashes, source inventories, code locations, endpoint catalogs, secret-scan results, and the authentication boundary matrix.

## CTF Exercise Design

| Evidence ID | Class | Sanitized observation |
|---|---|---|
| CTF-E01 | Exercise artifact | Written challenge specification defined one synthetic product and explicit authorization boundaries |
| CTF-E02 | Exercise artifact | Solution rubric separated server-side completion from frontend-only price changes and errors |
| CTF-E03 | Exercise artifact | Smoke-test plan covered rate limits, logging, flag rotation, synthetic delivery, and absence of real funds |

These items demonstrate secure exercise design; they are not presented as evidence of a current live vulnerability.

## Integrity Note

Git commits provide integrity and revision history for the public documents. They do not replace confidential validation of the original engagement records.

