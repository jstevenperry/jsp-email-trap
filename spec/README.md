# Email Trap POC — Specification Index

**Draft v0.3 (review draft).** Read `00-poc-charter.md` first. Authoritative behavioral requirements live **only** in `01-system-requirements.md`, `02-sprint-1.md`, and `03-sprint-2.md`. `04` contains test scenarios referencing IDs; `05` contains technical constraints; `06` governs the POC evaluation. Prior v0.1 and v0.2 packages are preserved separately.

## Scope by sprint
- **Sprint 1:** SMTP capture (including unknown recipients), current-assignment inbox access, shared addresses, persistent per-principal hiding, OIDC, visible attachment/raw-MIME downloads, portable deployment.
- **Sprint 2:** Direct-report read-only visibility, overlapping own-recipient hide entitlement, administrator-only dynamic dead-letter inspection, independent regression verification.
- **Candidate Sprint 3 (not specified yet):** Inline previews for supported attachment types; unsupported types remain download-only.

## v0.3 change log
1. Approved concurrent address assignment to multiple principals.
2. Approved persistent hidden-message state across loss and restoration of an address assignment.
3. Clarified direct-recipient hide entitlement for managers who also have read-only subordinate access.
4. Added Sprint 2 administrator-only dead-letter inspection for currently unassigned SMTP envelope recipients, including mixed-recipient messages, automatic reclassification, historical visibility, and no manual delivery.
5. Made Sprint 1 visible per-attachment and raw-MIME Download actions explicit.
6. Added corresponding acceptance scenarios and clarified independent QA test ownership.
7. Preserved D-01–D-08, including D-08 historical visibility under current assignments.

## Remaining pre-v1.0 work
Finalize operational SMTP/OIDC defaults and complete independent acceptance coverage for every requirement, including negative/boundary cases. Do not treat this review draft as a frozen implementation baseline.
