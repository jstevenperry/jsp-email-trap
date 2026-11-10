# Acceptance Criteria and Traceability
Version: 0.1 (draft) | Authority: behavioral verification scenarios, **not** requirement definitions

The source of each requirement is `01`, `02`, or `03`. This document references IDs without restating normative requirements. Scenarios below are illustrative baseline cases; before the v1.0 gate, generate complete automated coverage for every functional requirement, including negative paths and unresolved decisions.

## Sprint 1 scenarios
| Scenario | Requirement IDs | Given / When / Then |
|---|---|---|
| S1-001 | SYS-003, AUTH-001, AUTH-002, AUTH-003, AUTH-004, UI-001 | Given Alice authenticates through trusted OIDC and owns `alice@test.local`; when mail to that address is accepted; then Alice sees it in her inbox. |
| S1-002 | SYS-004, SYS-005, AUTH-005 | Given Alice and Bob have disjoint addresses and Bob has a message; when Alice requests Bob's message by ID or tries its attachment/raw-MIME URLs; then each request is denied without revealing content. |
| S1-003 | SYS-002, SMTP-005, DATA-003, AUTH-004 | Given one SMTP transaction has envelope recipients Alice and Bob; when it is accepted; then one message record exists and both users can view it. |
| S1-004 | SYS-006, DATA-001, DEP-002 | Given a message is stored; when the application container is recreated with the same volume; then the message remains available. |
| S1-005 | SYS-008, DEP-004 | Given Hanzo/AI services are unavailable; when the Compose application starts with its OIDC provider available; then capture, login, and inbox operations still work. |
| S1-006 | SMTP-006 | Given a message exceeds the configured size; when submitted via SMTP; then SMTP rejects it and no partial message is visible or stored. |
| S1-007 | SMTP-003, DATA-002, UI-003, UI-004, UI-005 | Given an email with plain text, HTML, and an attachment; when an authorized user opens it; then content is inspectable and MIME/attachment downloads preserve bytes; unsafe HTML does not execute. |
| S1-008 | AUTH-006 | Given no session or an invalid/expired credential; when protected endpoints are requested; then access is denied. |
| S1-009 | UI-006, DATA-003 | Given a shared message for Alice and Bob; when Alice hides it; then Bob retains visibility and the underlying message remains stored. **Pending D-03.** |
| S1-010 | SYS-001, SMTP-004 | Given SMTP mail is accepted; when processing completes; then no external SMTP delivery attempt occurs. |
| S1-011 | SYS-002 | Given MIME `To` says Alice but SMTP envelope recipient is Bob; when accepted; then entitlement follows Bob, not Alice. |
| S1-012 | AUTH-002, AUTH-003 | Given two identity providers issue the same `sub`, or a user changes their email claim; when logging in; then identity is resolved by `(iss,sub)` and does not automatically grant another mailbox. |

## Sprint 2 scenarios
| Scenario | Requirement IDs | Given / When / Then |
|---|---|---|
| S2-001 | ORG-001, ORG-003, ORG-008 | Given Alice directly manages Bob; when Bob receives mail; then Alice and Bob may view its detail and downloads. |
| S2-002 | ORG-004 | Given Alice manages Bob; when Bob requests Alice's exclusive message; then access is denied. |
| S2-003 | ORG-005 | Given Alice manages Bob and Bob manages Carol; when Alice requests Carol's exclusive message; then access is denied, while Bob may view it. |
| S2-004 | ORG-007 | Given Bob is removed as Alice's report; when Alice makes a subsequent request for Bob's exclusive message; then access is denied without re-login. |
| S2-005 | ORG-010 | Given Alice sees her own and Bob's messages; when she opens the inbox; then the source of each entitlement is distinguishable. |
| S2-006 | ORG-002, ORG-006 | Given seed data assigns Bob two direct managers; when configuration is validated; then the invalid relationship is rejected. |
| S2-007 | ORG-009, SYS-009 | Given Sprint 2 changes are deployed; when the frozen Sprint 1 suite runs; then every previously accepted scenario still passes. |

## Coverage completion checklist
- Expand positive, boundary, and negative cases for **every** requirement ID in `01`–`03`.
- Include SMTP protocol behavior, concurrency, persistence, OIDC state/nonce/PKCE, access by direct ID, and shared-recipient deletion semantics.
- Resolve D-01–D-08 and amend tests before freezing the Sprint 1/Sprint 2 baselines.
- Keep an independently owned black-box suite outside the agent's write permissions; record code-generated test coverage separately.

## Traceability convention
Every executable test must carry at least one requirement ID and scenario ID. An automated report shall list uncovered requirement IDs and distinguish `not tested`, `failed`, and `passed`. Requirement wording is never copied here as an alternative source of truth.
