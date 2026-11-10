# Email Trap Proof of Concept

This repository defines a requirements-driven proof of concept (POC) for building and incrementally evolving a full-stack email-capture application. It is primarily a **specification and evaluation repository**: the numbered documents describe intended product behavior, implementation constraints, acceptance scenarios, and how to assess the generated application. The specifications are not themselves an application implementation.

The experiment asks whether Hanzo can build, test, deploy, and maintain an application from human-readable requirements as the authoritative source of truth, without human-authored application patches or Hanzo/AI dependencies in the deployed runtime. It is intended to evaluate incremental, requirements-led development—not just one-shot code generation.

> **Specification status:** The current documents are version 0.5 review drafts. They describe the approved scope, but are not yet a frozen v1.0 implementation baseline. See [`spec/README.md`](spec/README.md) for the document index and remaining pre-baseline work.

## Product at a glance

The planned product is a multi-user SMTP capture server with a private browser inbox:

- It accepts and stores SMTP messages for inspection; it **never relays or delivers mail externally**.
- Users authenticate through a configurable OpenID Connect (OIDC) provider.
- Access is determined by the message's SMTP envelope recipients and application-managed address assignments—not just the visible `To` or `Cc` headers, and not automatically by a user's login email.
- Authorized users can inspect message content, download attachments, and download the original MIME message.
- A message may have multiple envelope recipients but is stored as one underlying message. Each user's visibility and hidden-message state are managed independently.
- Later sprints add narrowly scoped direct-manager visibility, administrator-only inspection of currently unassigned recipients, and safe inline previews of selected attachment formats.

SMTP acceptance means durable capture, not inbox delivery. A message addressed to an unassigned address remains stored but is not available in a regular user's inbox unless an authorized assignment later grants access.

## Planned increments

The POC is divided into three sprints. Each sprint builds on the previous one; new work must preserve previously accepted behavior unless a reviewed requirements change explicitly supersedes it.

### Sprint 1: Capture and private inbox

- Accept SMTP messages, including messages with multiple recipients, and preserve the original raw MIME data.
- Authenticate browser users with OIDC Authorization Code Flow with PKCE.
- Grant inbox access through explicitly configured trap-address assignments. One address may be assigned to multiple principals.
- Provide message details and original MIME and attachment downloads, while protecting every operation with server-side authorization.
- Allow users to hide messages for themselves without removing another user's access.
- Provide a portable Compose deployment with persistent message storage and a development Keycloak identity provider.

### Sprint 2: Organizational access and dead-letter inspection

- Add configured, direct manager/subordinate relationships. A manager can read messages addressed to themselves or their immediate direct reports, but access does not extend to indirect reports or peers.
- Keep manager-only access read-only; a manager may hide a message only when independently entitled through their own address.
- Add an administrator-only, read-only view of messages with currently unassigned envelope recipients.
- Recalculate address and dead-letter visibility from current configuration, so new assignments affect previously captured messages without retransmission.

### Sprint 3: Safe attachment previews

- Add inline previews for valid PDF, PNG, JPEG, GIF, WebP, and plain-text attachments.
- Keep downloads available for every attachment, including unsupported or unrenderable types.
- Prevent attachment scripts, active content, and remote resource loads from executing in previews, and enforce the same authorization rules for preview requests as for downloads.

## Technical direction

The approved reference stack is Python 3, FastAPI, `aiosmtpd`, SQLite, and a lightweight HTML/JavaScript frontend. The reference deployment uses OCI-compatible containers and Compose, with persistent storage; Docker Desktop is not required. Keycloak is the development/test OIDC provider, while issuer, client settings, secrets, ports, and operational limits are deployment-configurable.

The application must not require a Hanzo service, AI model, AI API, agent runtime, or AI-provider network connection to run. The SMTP listener is a trusted-network test trap, not an internet-facing mail server. See [`spec/5000-architecture-constraints.md`](spec/5000-architecture-constraints.md) for the full technical and operational constraints.

## Requirements and source of truth

Normative behavioral requirements have one authoritative home: shared requirements live in `1000`, and sprint-specific requirements live in `2000`, `2001`, or `2002`. Acceptance criteria and evaluation documents refer to those requirement IDs; they are not alternate definitions of product behavior.

| Document | Purpose |
|---|---|
| [`spec/0000-poc-charter.md`](spec/0000-poc-charter.md) | Experiment hypothesis, principles, sprint boundaries, measures, and exclusions |
| [`spec/1000-system-requirements.md`](spec/1000-system-requirements.md) | Shared cross-sprint behavioral requirements |
| [`spec/2000-sprint-1.md`](spec/2000-sprint-1.md) | SMTP capture, identity, authorization, inbox, and deployment requirements |
| [`spec/2001-sprint-2.md`](spec/2001-sprint-2.md) | Manager access and administrator dead-letter requirements |
| [`spec/2002-sprint-3.md`](spec/2002-sprint-3.md) | Attachment preview requirements |
| [`spec/4000-acceptance-criteria.md`](spec/4000-acceptance-criteria.md) | Acceptance scenarios and requirement traceability |
| [`spec/5000-architecture-constraints.md`](spec/5000-architecture-constraints.md) | Approved implementation, security, and operational boundaries |
| [`spec/6000-evaluation-protocol.md`](spec/6000-evaluation-protocol.md) | Sprint execution, independent QA, regression checks, and evidence recording |

When changing a requirement, update its authoritative document and record affected requirement IDs, acceptance scenarios, and sprint baseline. Keep implementation constraints in `5000`; do not duplicate behavioral requirements across documents.

## Evaluation approach

The POC is intended to be evaluated sprint by sprint:

1. Freeze and tag the applicable specification and independent acceptance-test baseline.
2. Require traceable tests before implementation of each requirement.
3. Keep the independent black-box acceptance harness protected from the implementation agent.
4. Run generated tests, independent acceptance and security checks, regression tests from earlier sprints, and clean-environment deployment checks.
5. Preserve prompts, changes, failures, results, deployment evidence, human interventions, and reported AI usage.

The target for each sprint is full passage of required independent scenarios, no unresolved critical security failures, no regressions, no human-written application patches, and no Hanzo/AI runtime dependency. These are evaluation goals, not claims that an implementation has already met them. The protocol is detailed in [`spec/6000-evaluation-protocol.md`](spec/6000-evaluation-protocol.md).

## Scope exclusions

This POC does not include outbound mail delivery, manual routing or redelivery of dead-letter messages, self-service account creation, enterprise SSO provisioning, transitive manager visibility, message search, production mail-server hardening, or AI features at runtime. Sprint 3 also excludes OCR, attachment editing, attachment search, and thumbnail generation. The charter lists the broader exclusions and success measures.
