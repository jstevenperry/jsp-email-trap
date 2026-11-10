# Sprint 2 — Organizational Authorization Requirements
Version: 0.2 (review draft) | Authority: Sprint 2 requirements only

## Outcome
Extend the Sprint 1 application to allow managers to see messages addressed to direct reports. All shared and Sprint 1 requirements continue to apply unless a change is explicitly recorded.

| ID | Requirement |
|---|---|
| ORG-001 | Maintain explicit direct-manager/subordinate relationships between application principals. |
| ORG-002 | Each principal shall have at most one direct manager; self-management and cycles are invalid (D-07). |
| ORG-003 | A manager shall be entitled to messages addressed to any trap address currently assigned to themselves or an immediate direct report, including previously captured messages (D-08). |
| ORG-004 | A subordinate shall not gain access to a manager's or peer's messages solely because of the reporting relationship. |
| ORG-005 | Managerial access shall not extend transitively to indirect reports. |
| ORG-006 | Reporting relationships shall be configurable through controlled seed/configuration data without application code changes. |
| ORG-007 | An updated reporting relationship shall affect authorization on the next protected request without requiring a new login. |
| ORG-008 | Managerial entitlements shall apply consistently to inbox listing, message detail, attachments, and raw MIME download. |
| ORG-009 | All Sprint 1 entitlements and denials shall remain effective unless explicitly superseded; no new access shall be granted through untrusted user-supplied fields. |
| ORG-010 | The UI shall distinguish messages accessed through a direct report from those accessed through the user's own addresses. |

## Authorization semantics
Manager status is derived from explicit direct-report relationships, not solely from an OIDC role claim. OIDC authenticates identity; application configuration governs mailbox entitlements. The rule in ORG-003 **extends** AUTH-004; it does not replace the original ownership entitlement. No administrative UI is required in Sprint 2.

## Approved decisions (D-06–D-08)
- **D-06:** Managers have read-only access to messages via direct-report entitlement. They cannot hide or delete a subordinate's message through that entitlement. Their own messages remain subject to Sprint 1 per-principal hide rules.
- **D-07:** Each principal has at most one direct manager; reject self-management and cycles.
- **D-08:** Managerial visibility follows current address assignments and current reporting relationships. A new manager can read a report's previously captured messages; a removed manager immediately loses that access, including historical messages.

## Clarification for v1.0
- When a message is accessible both through a manager's own address and through a subordinate's address, the manager retains their own-address hide privilege; the UI/API must distinguish the entitlement used for any mutation.

## Regression boundary
Sprint 2 must pass the complete frozen Sprint 1 independent acceptance suite plus the new Sprint 2 suite. No human-written code patches are allowed under the POC protocol.
