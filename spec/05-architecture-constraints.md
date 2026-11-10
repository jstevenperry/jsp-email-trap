# Architecture and Operational Constraints
Version: 0.4 (review draft) | Authority: technical constraints, not business rules

These are technical boundaries for generated implementation. Business behavior is defined only in `01`–`03`.

| ID | Constraint |
|---|---|
| ARC-001 | Reference stack: Python 3, FastAPI, `aiosmtpd`, SQLite, and a lightweight HTML/JavaScript frontend. No framework substitution without approval. |
| ARC-002 | Use standard OIDC Authorization Code Flow with PKCE, validate issuer/audience/signatures and login state/nonce as appropriate, and use secure HTTP-only session cookies. |
| ARC-003 | Reference identity provider is Keycloak in Compose; issuer, client configuration, and secrets must be externally configurable. |
| ARC-004 | All authorization checks occur in backend services for list, detail, MIME, attachment, hide, and administrative dead-letter operations; no trust in client-provided identity or recipient claims. |
| ARC-005 | Store raw MIME plus normalized envelope-recipient associations; support independent durable per-principal view state without duplicating the raw message; dead-letter is a query over current recipient assignments, not a second message store. |
| ARC-006 | Render HTML email only in a constrained sandbox/sanitized context with active content disabled and remote loads blocked by default. |
| ARC-007 | Use OCI-compatible images, Compose for local/reference deployment, and persistent volumes. No Docker Desktop requirement; Podman compatibility is a validation target, not assumed. |
| ARC-008 | Prefer non-root runtime, minimal image, secrets outside source, least-privilege storage, and private/trusted network bindings by default. |
| ARC-009 | Include readiness/health checks, structured operational logs without message-body leakage, and reproducible start/stop instructions. |
| ARC-010 | No Hanzo SDK, AI API, agent runtime, or network call to an AI provider in the deployed application. Development tooling may use Hanzo outside the runtime. |
| ARC-011 | Do not expose an unauthenticated message API or publicly accessible Keycloak admin console in the reference configuration. |

## Reference deployment topology
- `app`: HTTP UI/API (default container port 8080), SMTP capture (default 1025), SQLite persistent volume.
- `keycloak`: development/test OIDC provider with seeded test identities; use supported persistent settings for repeatable tests.
- Optional reverse proxy with HTTPS for remote deployment. Browser OIDC redirect URIs and issuer URLs must match the actual externally reachable origin; localhost-only settings are not sufficient for remote users.
- Expose SMTP only on a trusted interface/network; this is a test trap, not an internet-facing mail transfer agent.

## Engineering boundaries
Use standard SMTP/MIME libraries rather than writing protocol parsers from scratch. Handle malformed MIME defensively; do not execute attachments. Use migrations or explicit schema versioning between sprints. Prefer deterministic test fixtures and disposable test databases. Preserve original SMTP envelope recipient metadata even when MIME headers differ.

## Approved v0.4 operational defaults
The following are approved, testable technical constraints. Functional behavior remains authoritative in `01`–`03`.

| ID | Constraint |
|---|---|
| ARC-012 | SMTP supports standard `EHLO`/`HELO` and advertises `SIZE` and `8BITMIME`; do not advertise unsupported extensions. `SMTPUTF8`, `STARTTLS`, SMTP `AUTH`, and `PIPELINING` are not required. If a library implements additional extensions, only advertise and enable those that are verified by tests; do not implicitly enable public relay. |
| ARC-013 | Use standard SMTP response codes: `220` greeting, `250` accepted command/recipient and durable message acceptance, `354` start data, `221` quit; `500` unknown command, `501` malformed command/address, `503` invalid command sequence, `552` message too large, `451` transient storage/processing failure, `421` service unavailable or connection capacity reached. Unknown-but-syntactically-valid recipient addresses receive the same success response as assigned addresses; do not disclose address assignment via SMTP. A failed transaction shall not commit a partial message. |
| ARC-014 | Default SMTP limits: 20 concurrent connections, 60 seconds of connection inactivity, at most 100 envelope recipients per transaction, and 10 MiB (10,485,760 bytes) of message data excluding SMTP dot-stuffing. Limits are configurable without source edits. Excess recipient count shall fail without accepting the excess recipient; size overage shall return `552`. |
| ARC-015 | Authenticated web sessions have an eight-hour absolute lifetime and 30-minute inactivity timeout, both configurable; authenticated activity refreshes only the inactivity deadline. Session expiry requires reauthentication. Logout invalidates server-side session state, removes the browser session cookie, and redirects to login; identity-provider-wide single logout is not required. |
| ARC-016 | Use a documented consistent SQLite online backup/snapshot and restore procedure; verify recovery with captured messages and envelope-recipient associations. Do not simply copy a live database file without ensuring consistency. No scheduled backup service is required. |
| ARC-017 | Retain captured messages indefinitely for the POC; no automatic age-based purging. Administrative teardown/removal of persistent storage is an explicit out-of-band operation, not an inbox hide action. |
| ARC-018 | Normalize and compare trap addresses using ASCII-only syntax: preserve the case of the local part, compare domain names case-insensitively, and perform no implicit plus-tag stripping, alias expansion, Unicode normalization, or SMTPUTF8 handling. |
| ARC-019 | SMTP must bind only to an explicitly configured trusted/private interface or network and never be published to the public internet by the reference deployment. Local browser access may use HTTP on loopback; remote browser access, OIDC redirects, and callbacks require HTTPS with matching externally reachable issuer/origin configuration. Keycloak administrative endpoints must not be publicly exposed. |
| ARC-020 | SMTP, HTTP, OIDC, timeout, and limit settings shall be configurable through deployment configuration/environment variables without source modification; secrets must not be committed to the source repository. |

## Operational interpretation
A final SMTP `250` after message `DATA` acknowledges durable capture by the trap, **not** delivery to a mailbox or external recipient. Session and message limits apply to SMTP submission independently of whether a recipient is assigned. For `8BITMIME`, preserve accepted octets in stored raw MIME. A 100-recipient limit counts accepted envelope recipient commands within one transaction. A connection rejected at capacity may receive `421` if the server can respond before closing.

## Remaining pre-v1.0 review
Expand the independent test harness for every requirement and operational constraint, validate library-supported SMTP extension advertisement and exact failure behavior, and confirm the reference Compose network bindings in a clean deployment. These are verification tasks, not unresolved policy choices.
