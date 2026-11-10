# Shared System Requirements
Version: 0.3 (review draft) | Authority: shared cross-sprint requirements only

Each ID below is defined **only here**. Sprint documents and acceptance tests reference these IDs.

| ID | Requirement |
|---|---|
| SYS-001 | The system shall capture accepted SMTP messages and shall never relay or deliver them to external mail systems. |
| SYS-002 | Access to captured messages shall be determined using the SMTP envelope recipient addresses, not solely the MIME `To`/`Cc` headers. |
| SYS-003 | Browser users shall authenticate through a configurable OpenID Connect (OIDC) provider. |
| SYS-004 | All protected operations shall enforce authorization on the server, regardless of UI behavior. |
| SYS-005 | Users shall access only messages, message content, and attachments authorized under the currently effective access rules. |
| SYS-006 | Accepted message data and recipient associations shall survive application container restarts and recreation when the same persistent volume is used. |
| SYS-007 | The application shall be deployable as OCI-compatible containers using a Compose configuration without requiring Docker Desktop. |
| SYS-008 | No Hanzo service, AI model, AI API, or development-platform connection shall be required to run the deployed application. |
| SYS-009 | Accepted requirements from earlier sprints remain in force unless explicitly superseded through a reviewed requirements change. |
| SYS-010 | Every functional requirement shall have traceable acceptance evidence, including negative cases for access-control requirements. |

## Interpretation
“User” refers to an authenticated application principal. “Trap address” refers to an email address assigned to that principal by application configuration. The user's OIDC login email is not automatically a trap-address entitlement. Message visibility is separate from SMTP acceptance.

## Change control
Changes to this file require recording the affected IDs, acceptance scenarios, and sprint baseline. Do not duplicate shared requirement text in sprint documents.
