# Acceptance Criteria and Traceability
Version: 0.3 (review draft) | Authority: behavioral verification scenarios, **not** requirement definitions

The source of each requirement is `01`, `02`, or `03`. This document references IDs without restating normative requirements. Scenarios below are a review-draft baseline; before the v1.0 gate, complete automated coverage for every functional requirement, including negative and boundary paths.

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
| S1-009 | UI-006, DATA-003 | Given a shared message for Alice and Bob; when Alice hides it; then Bob retains visibility and the underlying message remains stored. **D-03 approved.** |
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

## Additional scenarios for approved decisions
| Scenario | Requirement IDs | Given / When / Then |
|---|---|---|
| S1-013 | SMTP-002, AUTH-003, AUTH-004 | Given mail for an unassigned address was captured yesterday; when that address is assigned to Alice today; then Alice can view yesterday's message without redelivery. |
| S1-014 | AUTH-003, AUTH-004, AUTH-005 | Given Alice currently owns an address and can see its historical messages; when the assignment is removed; then Alice cannot access those messages by list, ID, or download. |
| S1-015 | AUTH-003, AUTH-004, AUTH-005 | Given an address previously assigned to Alice is reassigned to Bob; when Bob logs in; then Bob can access all previously captured messages for that address and Alice cannot access them solely through the old assignment. |
| S1-016 | UI-006 | Given Alice hid a message and Bob can also see it; when Bob views his inbox; then the message is visible to Bob and hidden from Alice. |
| S2-008 | ORG-003, ORG-007 | Given Bob received a message before Alice became his manager; when Alice becomes Bob's direct manager; then Alice can view that earlier message. |
| S2-009 | ORG-007, ORG-008 | Given Alice previously managed Bob; when the relationship is removed; then Alice loses all access granted solely through Bob, including access to historical attachments and raw MIME. |
| S2-010 | ORG-003, ORG-008 | Given Alice is authorized only as Bob's manager; when Alice attempts to hide Bob's message; then the mutation is denied and Bob's visibility is unchanged. |
| S2-011 | ORG-002 | Given self-management or a cycle in the relationship seed; when configuration is validated; then it is rejected. |

## v0.3 additional acceptance scenarios
| Scenario | Requirement IDs | Given / When / Then |
|---|---|---|
| S1-017 | AUTH-003, AUTH-004, UI-001 | Given Alice and Bob concurrently share `shared@test.local`; when a message arrives for that address; then both can see it independently. |
| S1-018 | UI-006, AUTH-004 | Given Alice hides a shared message, loses the address assignment, and later regains it; when she views her inbox; then that message remains hidden for Alice while Bob's view is unchanged. |
| S1-019 | UI-004, UI-005, DATA-002 | Given a message has two attachments; when an authorized user opens its detail; then each attachment has a visible Download action and the raw MIME has its own visible Download action; downloaded bytes match originals. |
| S1-020 | AUTH-005, UI-005 | Given an unauthorized user knows a valid attachment or MIME URL; when they request it directly; then content is denied. |
| S2-012 | ORG-011, UI-006 | Given Alice is both a direct envelope recipient and Bob's manager for one message; when Alice hides it; then only Alice's inbox hides it, while Bob's inbox remains unchanged. |
| S2-013 | DLQ-001, DLQ-002, DLQ-003 | Given an unknown address receives mail; when a configured admin opens dead-letter; then the message and unassigned recipient are listed and the admin can inspect its detail and original bytes. |
| S2-014 | DLQ-003 | Given a regular user or non-admin manager knows a dead-letter message ID; when they request admin list, detail, attachment, or MIME endpoints; then every request is denied. |
| S2-015 | DLQ-004, AUTH-004 | Given an unassigned address has historical mail in dead-letter; when the address is assigned to Alice; then the address no longer qualifies as dead-letter and Alice can view the historical message without retransmission. |
| S2-016 | DLQ-005, DATA-003 | Given one message has assigned recipient Alice and unassigned recipient Nobody; when it is accepted; then Alice can view it, the admin sees Nobody in dead-letter, and only one underlying message record exists. |
| S2-017 | DLQ-005, DLQ-004 | Given a message has two unassigned recipients; when only one is assigned; then dead-letter retains only the other unassigned recipient and the new assignee sees the message. |
| S2-018 | DLQ-006, SYS-001 | Given an admin opens dead-letter; when inspecting available actions; then no address assignment, forwarding, redelivery, or external-delivery action is available. |
| S2-019 | DLQ-003 | Given a principal authenticates successfully but is not configured as an admin; when they forge an admin role or client-provided identity; then administrative access remains denied. |

## Additional traceability scenarios for previously uncovered IDs
| Scenario | Requirement IDs | Given / When / Then |
|---|---|---|
| S1-021 | SMTP-001, DEP-003 | Given a configured non-default SMTP port; when a client connects on that port; then the server accepts a valid SMTP transaction without source edits. |
| S1-022 | DATA-004, UI-002 | Given a captured message; when an authorized user opens the inbox; then sender, envelope recipients, subject, and UTC receipt timestamp are displayed accurately. |
| S1-023 | DEP-001, SYS-007 | Given a clean host with compatible container tooling; when the reference Compose setup starts; then the application and development Keycloak provider are available without Docker Desktop. |
| S1-024 | DEP-003 | Given external OIDC client settings and application secrets; when configuration is changed and the application restarted; then it uses the new settings without modifying source code. |
| S1-025 | SYS-010 | Given the Sprint 1 test report; when it is reviewed; then each applicable functional requirement has traceable acceptance evidence and access-control denials have negative cases. |

## Coverage completion checklist
- Expand positive, boundary, and negative cases for **every** requirement ID in `01`–`03`.
- Include SMTP protocol behavior, concurrency, persistence, OIDC state/nonce/PKCE, access by direct ID, and shared-recipient deletion semantics.
- D-01–D-08 and the v0.3 shared-address, persistent-hide, overlapping-entitlement, and dead-letter decisions are approved; expand all applicable tests before freezing the Sprint 1/Sprint 2 baselines.
- Keep an independently owned black-box suite outside the agent's write permissions; record code-generated test coverage separately.

## Traceability convention
Every executable test must carry at least one requirement ID and scenario ID. An automated report shall list uncovered requirement IDs and distinguish `not tested`, `failed`, and `passed`. Requirement wording is never copied here as an alternative source of truth.
