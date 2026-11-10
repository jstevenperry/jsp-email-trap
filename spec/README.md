# Email Trap POC — Specification Index

**v1.0 — approved and frozen requirements baseline for Sprints 1–3.** All agreed behavior is codified in the numbered requirements. This package freezes what must be built; executable independent QA tests and deployment verification are separate sprint-readiness tasks.

## Where requirements live
- `0000-poc-charter.md`: experiment goals, sprint boundaries, and success criteria.
- `1000-system-requirements.md`: shared cross-sprint behavioral requirements.
- `2000-sprint-1.md`: Sprint 1 behavioral requirements, including capture, OIDC, inbox, and attachment downloads.
- `2001-sprint-2.md`: Sprint 2 behavioral requirements, including manager access and admin dead-letter inspection.
- `2002-sprint-3.md`: **new** Sprint 3 behavioral requirements for inline attachment previews.
- `4000-acceptance-criteria.md`: test scenarios and traceability, not alternative requirement definitions.
- `5000-architecture-constraints.md`: approved technical and operational implementation constraints.
- `6000-evaluation-protocol.md`: execution, independent QA, regression, and evidence protocol.

Each normative behavioral requirement has one authoritative home (`1000`, `2000`, `2001`, or `2002`); other files reference its ID. Technical constraints are defined in `5000`.

## Approved sprint scope
- **Sprint 1:** SMTP capture, unknown-recipient storage, OIDC, address-based inboxes, shared addresses, persistent per-principal hiding, visible attachment and MIME downloads, portable deployment.
- **Sprint 2:** Read-only direct-report manager access, own-recipient hide entitlement, administrator-only dynamic dead-letter inspection, historical visibility under current address assignments.
- **Sprint 3:** Inline PDF, PNG, JPEG, GIF, WebP, and plain-text previews; exact unsupported/unrenderable fallback message; secure preview authorization; downloads retained.

## v1.0 baseline provenance
1. Promoted attachment preview from a candidate idea to a defined Sprint 3 with six new PREV requirements.
2. Added 11 Sprint 3 acceptance scenarios, including unsupported and malformed formats, access-control attacks, and full Sprint 1–2 regression.
3. Added three preview security/implementation constraints (ARC-021–ARC-023).
4. Updated charter, acceptance coverage, and evaluation protocol for three sprints.
5. Consolidated agreed behavior into numbered requirements; removed redundant decision identifiers and decision summaries. Clarified review drafts, verification tasks, and frozen baselines.

## Work remaining before sprint execution
Implement and freeze independent executable acceptance tests for each sprint, and verify SMTP library behavior, network bindings, and preview isolation as applicable. These are **test/engineering execution-readiness tasks**, not unresolved business decisions or changes to v1.0.

**Change control:** This package is the frozen baseline. Record its Git commit and tag (suggested tag: `email-trap-spec-v1.0`). Changes to requirements or constraints after this point require an explicitly versioned amendment, impact analysis, and updated traceability. The ZIP archive alone does not create a Git tag.

Version 1.0 is derived from the reviewed, four-digit-numbered v0.5 package; earlier archives remain separate.

## Document numbering convention

Four-digit prefixes group documents by purpose and provide stable, sortable names in file stores such as SharePoint:

| Range | Purpose |
|---|---|
| `0000`–`0999` | Introduction, charter, and project guidance |
| `1000`–`1999` | Shared system requirements |
| `2000`–`3999` | Sprint specifications (`2000` = Sprint 1, `2001` = Sprint 2, `2002` = Sprint 3) |
| `4000`–`4999` | Acceptance criteria and verification specifications |
| `5000`–`5999` | Architecture and operational constraints |
| `6000`–`6999` | Evaluation protocols and evidence guidance |

The `2000`–`3999` range has 2,000 available document numbers, sufficient for up to 2,000 sprint specification files when using one number per sprint. Prefixes categorize documents; version control remains separate from the filename.
