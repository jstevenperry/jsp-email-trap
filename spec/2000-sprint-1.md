# Sprint 1 — Functional Requirements
Version: 0.5 (review draft) | Authority: Sprint 1 requirements only

## Outcome
A working multi-user email trap with SMTP capture, OIDC login, private browser inboxes, and portable Compose deployment. Shared requirements in `1000-system-requirements.md` also apply.

### SMTP
| ID | Requirement |
|---|---|
| SMTP-001 | Listen for SMTP on a configurable port, default 1025. |
| SMTP-002 | Accept syntactically valid envelope recipient addresses, including currently unassigned addresses; accepted messages remain inaccessible until an authorized address assignment exists. |
| SMTP-003 | Preserve the accepted raw MIME message, including headers, multipart bodies, and attachments. |
| SMTP-004 | Do not offer or perform outbound SMTP relay. See SYS-001. |
| SMTP-005 | Support multiple SMTP envelope recipients for one message. |
| SMTP-006 | Reject messages larger than a configurable maximum, default 10 MiB (10,485,760 bytes of SMTP message data excluding dot-stuffing), without partially committing them; see ARC-014. |

### Data
| ID | Requirement |
|---|---|
| DATA-001 | Persist accepted messages and their envelope-recipient associations. |
| DATA-002 | Make original raw MIME available for authorized download. |
| DATA-003 | Store one underlying message record for a multi-recipient SMTP transaction. |
| DATA-004 | Record receipt timestamps in UTC. |

### Identity and authorization
| ID | Requirement |
|---|---|
| AUTH-001 | Support OIDC Authorization Code Flow with PKCE for interactive browser login. |
| AUTH-002 | Identify the principal by the combination of trusted OIDC issuer (`iss`) and subject (`sub`), not by email claim alone. |
| AUTH-003 | Associate each principal with one or more explicitly assigned trap addresses through administrator-controlled seed/configuration data; the same address may be assigned to multiple principals; no self-service assignment. |
| AUTH-004 | Permit access to a message when at least one of its envelope recipient addresses is currently assigned to the principal, including messages captured before assignment; subject to per-user hidden status. |
| AUTH-005 | Deny unauthorized access to inbox listings, message detail, attachments, and raw MIME downloads, including direct-ID requests. |
| AUTH-006 | Deny protected API access without a valid authenticated session and reject invalid/expired/untrusted tokens. |

### Browser interface
| ID | Requirement |
|---|---|
| UI-001 | Provide an authenticated browser inbox listing only authorized, non-hidden messages. |
| UI-002 | Display sender, envelope recipient(s), subject, and UTC receipt timestamp for messages visible to the user. |
| UI-003 | Display plain-text and safely sandboxed/sanitized HTML bodies without executing scripts or fetching remote resources by default. |
| UI-004 | Display message headers and attachment names, media types, and sizes. |
| UI-005 | Present a visible Download action for each listed attachment and a visible raw-MIME download action in the message-detail interface; downloads shall preserve original bytes for authorized users. |
| UI-006 | Allow a user to hide a message in their own inbox view without removing access for other entitled users; hidden state is per principal, not per address, and persists across removal and later restoration of address assignments. |

### Deployment
| ID | Requirement |
|---|---|
| DEP-001 | Supply a reference Compose setup containing the application and a development Keycloak OIDC provider. |
| DEP-002 | Persist captured message data on a named or bind-mounted volume. |
| DEP-003 | Configure SMTP/HTTP ports, trusted OIDC issuer/client settings, and application secrets without editing application source. |
| DEP-004 | Support deployment on a remote Linux host using standard container tooling, without proprietary runtime software. |

## Before v1.0
Complete independent acceptance-test coverage and verify the approved SMTP, OIDC, and deployment constraints in `5000` and `4000`.
