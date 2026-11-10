# Email Trap POC — Specification Index

**Draft v0.4 (review draft).** Read `00-poc-charter.md` first. Authoritative behavioral requirements live **only** in `01-system-requirements.md`, `02-sprint-1.md`, and `03-sprint-2.md`. `04` contains test scenarios referencing IDs; `05` contains technical constraints; `06` governs the POC evaluation. Prior v0.1 and v0.2 packages are preserved separately.

## Scope by sprint
- **Sprint 1:** SMTP capture (including unknown recipients), current-assignment inbox access, shared addresses, persistent per-principal hiding, OIDC, visible attachment/raw-MIME downloads, portable deployment.
- **Sprint 2:** Direct-report read-only visibility, overlapping own-recipient hide entitlement, administrator-only dynamic dead-letter inspection, independent regression verification.
- **Candidate Sprint 3 (not specified yet):** Inline previews for supported attachment types; unsupported types remain download-only.

## v0.3 change log (carried forward)
1. Approved concurrent address assignment to multiple principals.
2. Approved persistent hidden-message state across loss and restoration of an address assignment.
3. Clarified direct-recipient hide entitlement for managers who also have read-only subordinate access.
4. Added Sprint 2 administrator-only dead-letter inspection for currently unassigned SMTP envelope recipients, including mixed-recipient messages, automatic reclassification, historical visibility, and no manual delivery.
5. Made Sprint 1 visible per-attachment and raw-MIME Download actions explicit.
6. Added corresponding acceptance scenarios and clarified independent QA test ownership.
7. Preserved D-01–D-08, including D-08 historical visibility under current assignments.

## v0.4 change log
1. Approved SMTP extension baseline (`EHLO`, `SIZE`, `8BITMIME`) and standard success/failure response codes, including uniform acceptance of unassigned recipients.
2. Approved SMTP connection, inactivity, recipient-count, and 10 MiB message-data limits, with configurable values.
3. Approved eight-hour absolute and 30-minute idle web sessions; local logout invalidates the server-side session.
4. Approved consistent SQLite manual backup/restore and indefinite POC retention.
5. Reaffirmed ASCII-only address comparison and private SMTP / HTTPS remote browser exposure policy.
6. Added operational acceptance scenarios OP-001–OP-012 referencing the approved architecture constraints ARC-012–ARC-020.

## Remaining pre-v1.0 work
Complete independent acceptance coverage for every requirement and validate the operational defaults against the chosen libraries and deployment environment. This remains a review draft, not a frozen implementation baseline.
