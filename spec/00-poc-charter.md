# Email Trap POC — Charter
Version: 0.3 (review draft) | Status: Proposed | Scope: Sprints 1–2

## Hypothesis
Can Hanzo build, test, deploy, and incrementally maintain a full-stack application from human-readable requirements as the authoritative source of truth, with zero Hanzo/AI runtime dependencies and no human-authored application patches?

## Product and experiment
The product is a multi-user SMTP capture server with a private browser inbox. Sprint 1 delivers a usable application; Sprint 2 adds direct-manager visibility and administrator-only dead-letter inspection. The experiment tests requirements-driven evolution, not simply one-shot code generation. Future execution of the same baseline on GitHub Fusion is outside this POC.

## Principles
1. Requirements have **one authoritative location**: shared requirements in `01`, sprint-specific requirements in `02` or `03`; tests and constraints reference their IDs without redefining them.
2. Write executable tests before implementing each requirement; preserve previous accepted behavior unless an explicit change is approved.
3. Human participants may refine specifications, review results, and provide environment configuration, but do not patch generated application code.
4. Independently controlled black-box checks supplement AI-generated tests.
5. Commit and tag the specifications, generated code, test results, and deployment configuration at each sprint boundary.

## Scope and gates
- Sprint 1: SMTP capture, MIME preservation, OIDC authentication, per-address segregation, inbox and downloads, persistent Compose deployment.
- Sprint 2: direct-report relationships, manager visibility, and administrator-only dead-letter inspection, with Sprint 1 regression protection.
- Sprint 1 exit: independently deployable and usable, acceptance tests pass, no critical authorization defects.
- Sprint 2 exit: new access rules pass and Sprint 1 tests still pass.

## Measures
Capture: requirements coverage, independently verified pass/fail, security failures, regression count, manual implementation edits (target 0), human interventions, elapsed time, AI model/API usage cost, deployment repeatability, and runtime dependency audit. These are goals, not guaranteed outcomes.

## Scope exclusions
No outbound delivery, manual routing of dead-letter messages, inline attachment previews (candidate Sprint 3), self-service account creation, enterprise SSO provisioning, nested managerial visibility, message search, production mail-server hardening, or AI features at runtime. Deferred decisions are in the sprint specifications.

## Artifacts
`01` shared requirements; `02` Sprint 1; `03` Sprint 2; `04` acceptance scenarios; `05` technical constraints; `06` evaluation protocol. This charter governs the experiment but is not a second source of application requirements.
