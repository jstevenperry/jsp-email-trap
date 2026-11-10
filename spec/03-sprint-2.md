# Sprint 2 — Organizational Authorization Requirements
Version: 0.4 (review draft) | Authority: Sprint 2 requirements only

## Outcome
Extend the Sprint 1 application to allow managers to see messages addressed to direct reports and administrators to inspect mail for currently unassigned recipients. All shared and Sprint 1 requirements continue to apply unless a change is explicitly recorded.

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
| ORG-011 | A manager with independent direct-recipient entitlement to a message may hide that message for themselves; managerial entitlement alone shall never authorize hiding. |

## Authorization semantics
Manager status is derived from explicit direct-report relationships, not solely from an OIDC role claim. OIDC authenticates identity; application configuration governs mailbox entitlements. The rule in ORG-003 **extends** AUTH-004; it does not replace the original ownership entitlement. No address-assignment or organization-management UI is required in Sprint 2; the dead-letter inspection UI is required.

## Approved decisions (D-06–D-08)
- **D-06:** Managers have read-only access to messages via direct-report entitlement. They cannot hide or delete a subordinate's message through that entitlement. Their own messages remain subject to Sprint 1 per-principal hide rules.
- **D-07:** Each principal has at most one direct manager; reject self-management and cycles.
- **D-08:** Managerial visibility follows current address assignments and current reporting relationships. A new manager can read a report's previously captured messages; a removed manager immediately loses that access, including historical messages.

## Administrator dead-letter inspection — Sprint 2
| ID | Requirement |
|---|---|
| DLQ-001 | Provide an administrator-only dead-letter view of stored messages having at least one SMTP envelope recipient address that is currently unassigned to every principal. |
| DLQ-002 | For each dead-letter entry, show its currently unassigned envelope recipient address or addresses, sender, subject, and UTC receipt timestamp, and allow authorized administrators to inspect message detail, attachment downloads, and original raw MIME. |
| DLQ-003 | Grant dead-letter inspection only to explicitly configured administrator principals identified by trusted `(iss,sub)` identity; ordinary users and managers without that grant shall be denied dead-letter list, detail, attachment, and MIME access through the administrative interface, including direct-ID requests. |
| DLQ-004 | Reevaluate dead-letter classification using current address assignments on each protected request; assigning an address removes that recipient from the dead-letter view and makes its previously captured messages visible through normal address entitlements, without retransmission or manual routing. |
| DLQ-005 | Classify dead-letter status per envelope recipient association, not exclusively per stored message; a single multi-recipient message may be visible to authorized inbox users and to administrators for its unassigned recipients, without duplicating the underlying message. |
| DLQ-006 | The dead-letter interface shall be read-only; it shall not assign addresses, forward messages, trigger external delivery, or expose a manual redelivery action. |

## Approved clarifications
- **Overlapping entitlements:** Direct-recipient entitlement permits a principal-specific hide even if that principal is also a manager of another recipient; managerial entitlement by itself remains read-only (ORG-011).
- **Dead-letter terminology:** SMTP acceptance means captured and stored, not delivered to a user inbox. The dead-letter view is a dynamic administrative inspection view, not a separate SMTP delivery destination or a permanent quarantine.
- **Admin configuration:** Administrator identities are provisioned through controlled seed/configuration data; OIDC authentication alone or a client-supplied role does not grant administrator access.
- **D-08 consistency:** Historical messages are visible to newly assigned recipients, and no longer qualify as dead-letter for those now-assigned addresses.

## Regression boundary
Sprint 2 must pass the complete frozen Sprint 1 independent acceptance suite plus the new Sprint 2 suite. No human-written code patches are allowed under the POC protocol.
