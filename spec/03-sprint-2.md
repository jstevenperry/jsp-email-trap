# Sprint 2 — Organizational Authorization Requirements
Version: 0.1 (draft) | Authority: Sprint 2 requirements only

## Outcome
Extend the Sprint 1 application to allow managers to see messages addressed to direct reports. All shared and Sprint 1 requirements continue to apply unless a change is explicitly recorded.

| ID | Requirement |
|---|---|
| ORG-001 | Maintain explicit direct-manager/subordinate relationships between application principals. |
| ORG-002 | Each principal shall have at most one direct manager in this sprint. |
| ORG-003 | A manager shall be entitled to messages addressed to any trap address assigned to themselves or an immediate direct report. |
| ORG-004 | A subordinate shall not gain access to a manager's or peer's messages solely because of the reporting relationship. |
| ORG-005 | Managerial access shall not extend transitively to indirect reports. |
| ORG-006 | Reporting relationships shall be configurable through controlled seed/configuration data without application code changes. |
| ORG-007 | An updated reporting relationship shall affect authorization on the next protected request without requiring a new login. |
| ORG-008 | Managerial entitlements shall apply consistently to inbox listing, message detail, attachments, and raw MIME download. |
| ORG-009 | All Sprint 1 entitlements and denials shall remain effective unless explicitly superseded; no new access shall be granted through untrusted user-supplied fields. |
| ORG-010 | The UI shall distinguish messages accessed through a direct report from those accessed through the user's own addresses. |

## Authorization semantics
Manager status is derived from explicit direct-report relationships, not solely from an OIDC role claim. OIDC authenticates identity; application configuration governs mailbox entitlements. The rule in ORG-003 **extends** AUTH-004; it does not replace the original ownership entitlement. No administrative UI is required in Sprint 2.

## Decisions to resolve before implementation
- **D-06 Manager deletion:** Proposed: hiding a shared message is principal-specific; a manager hiding a report's message must not hide it for the report or another manager.
- **D-07 Relationship integrity:** Proposed: reject self-management and cycles; enforce unique direct manager.
- **D-08 Unassigned recipient visibility:** If Sprint 1 permits unknown-recipient capture, confirm that later address assignment makes previously captured messages visible to newly entitled users.

## Regression boundary
Sprint 2 must pass the complete frozen Sprint 1 independent acceptance suite plus the new Sprint 2 suite. No human-written code patches are allowed under the POC protocol.
