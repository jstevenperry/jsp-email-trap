# Architecture and Operational Constraints
Version: 0.1 (draft) | Authority: technical constraints, not business rules

These are technical boundaries for generated implementation. Business behavior is defined only in `01`–`03`.

| ID | Constraint |
|---|---|
| ARC-001 | Reference stack: Python 3, FastAPI, `aiosmtpd`, SQLite, and a lightweight HTML/JavaScript frontend. No framework substitution without approval. |
| ARC-002 | Use standard OIDC Authorization Code Flow with PKCE, validate issuer/audience/signatures and login state/nonce as appropriate, and use secure HTTP-only session cookies. |
| ARC-003 | Reference identity provider is Keycloak in Compose; issuer, client configuration, and secrets must be externally configurable. |
| ARC-004 | All authorization checks occur in backend services for list, detail, MIME, attachment, and deletion operations; no trust in client-provided identity or recipient claims. |
| ARC-005 | Store raw MIME plus normalized envelope-recipient associations; support independent per-principal view state without duplicating the raw message. |
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

## Unresolved implementation detail
Before freezing v1.0, define supported SMTP extensions, SMTP error codes, session limits, OIDC session lifetime/logout behavior, backup/retention, address normalization, and network exposure policy. These details may require additional requirements if they affect user-visible behavior.
