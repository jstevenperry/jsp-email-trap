# Hanzo POC Evaluation Protocol
Version: 0.4 (review draft) | Authority: experiment procedure and evidence

## Purpose
Measure whether Hanzo can generate and incrementally maintain the application from platform-neutral specifications. Do not confuse passing AI-authored tests with independent proof of correctness.

## Pre-run preparation
1. Freeze and Git-tag the requirements package for Sprint 1; confirm approved D-01–D-05 and Sprint 1 shared-address/hidden-state rules.
2. Prepare a separate black-box acceptance harness, fixtures, and security checks (including OP-001–OP-012 for the approved operational defaults); restrict Hanzo from editing it.
3. Record Hanzo beta version, models, configuration, token/cost reporting availability, host/container environment, and time budget.
4. Create a clean repository and preserve all prompts and generated changes.

## Sprint 1 protocol
1. Provide charter, shared requirements, Sprint 1 requirements, acceptance criteria, and constraints.
2. Require traceable tests before implementation; record order of creation.
3. Permit Hanzo to generate application, tests, Compose, and deployment instructions.
4. Permit corrections through requirements clarifications or agent instructions, not human implementation patches.
5. Run AI-generated tests, then independently controlled black-box tests, security checks, and fresh-environment deployment.
6. Record outcomes and tag the Sprint 1 baseline.

## Sprint 2 protocol
1. Freeze Sprint 1 baseline; confirm approved D-06–D-08 and Sprint 2 dead-letter/admin rules; tag Sprint 2 spec changes.
2. Supply the incremental Sprint 2 requirements plus the unchanged baseline, rather than requesting a full rewrite.
3. Require new tests first; preserve existing test suite and generated history.
4. Repeat all Sprint 1 and Sprint 2 acceptance and deployment checks.
5. Compare deltas, regressions, effort, and independent results; tag Sprint 2 outcome.

## Evidence ledger (one row per run)
| Field | Record |
|---|---|
| Run ID / sprint / spec commit | Immutable identifiers |
| Hanzo configuration / model(s) | Versions and settings |
| Start / end / elapsed time | Timestamps |
| Human interventions | Count, reason, text, and category |
| Manual implementation patches | Count and diff; success target = 0 |
| AI-generated test results | Passed/failed/skipped |
| Independent test results | Passed/failed/skipped, evidence links |
| Requirement coverage | Verified IDs / applicable IDs |
| Regressions | Previously passing cases now failing |
| Deployment result | Clean build, Compose startup, remote Linux, Podman if tested |
| AI cost | Reported token/API/compute charges; mark unavailable when unknown |
| Runtime audit | Dependency scan, network observations, no AI calls |
| Defects and exceptions | Severity, reproduction, resolution |

## Success and failure
Success target for each sprint: 100% of required independent acceptance scenarios passing, zero unresolved critical security failures, zero regressions at sprint completion, zero human-written application patches, and no Hanzo/AI runtime dependency. A failed target is a measured POC outcome, not a reason to alter independent tests to obtain a pass.

## Independent QA interpretation
Independent black-box tests may be authored by QA or another reviewer; the key requirement is independent ownership and protection from edits by the implementation agent. Freeze the harness and expected results before execution, and preserve negative/edge cases that are not disclosed as implementation examples.

## Independence safeguards
The AI may generate its own tests but may not edit the independent suite, change expected outcomes, or suppress failures. Humans may correct ambiguous requirements with documented revisions. Record any unavoidable manual environmental setup separately from manual code changes. Preserve failures and failed iterations, not only the final successful run.

## Future comparison
Retain identical platform-neutral spec commits, fixtures, external acceptance harness, and evaluation fields for possible later GitHub Fusion execution. Record environmental differences and avoid claiming controlled equivalence when tools or models differ.
