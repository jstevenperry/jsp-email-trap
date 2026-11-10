# Sprint 3 — Inline Attachment Preview Requirements
Version: 1.0 (frozen baseline) | Authority: Sprint 3 behavioral requirements only

## Outcome
Extend the existing message-detail experience with safe, authorized inline previews of common attachment types. Sprint 1 attachment downloads and Sprint 2 organizational/dead-letter authorization remain unchanged. This sprint adds previews, not a new attachment storage or delivery system.

| ID | Requirement |
|---|---|
| PREV-001 | Display a visible **Preview** action for each attachment in an authorized message-detail view. Selecting it shall attempt an inline preview without requiring the user to download the attachment. |
| PREV-002 | Support inline preview of valid PDF (`application/pdf`), PNG (`image/png`), JPEG (`image/jpeg`), GIF (`image/gif`), WebP (`image/webp`), and plain text (`text/plain`) attachments. |
| PREV-003 | For an unsupported attachment type or an attachment that cannot be safely rendered, display the exact message **Preview cannot be rendered for this type** rather than a blank or broken viewer. |
| PREV-004 | Render previews safely: do not execute attachment scripts or other active content, do not load remote resources from attachment content, and display plain text as text rather than executable markup. |
| PREV-005 | Preserve Sprint 1 visible Download actions for every attachment, including unsupported or unrenderable preview types. Previewing shall not change stored attachment or raw-MIME bytes. |
| PREV-006 | Enforce the same backend message/attachment authorization for preview as for download, including ordinary recipients, direct-report managers, and dead-letter administrators; deny direct preview URL/ID access without entitlement. |

## Interpretation and scope
- The preview type is determined using the attachment's declared media type together with validation that the content is safe and renderable as that type. A misleading filename or MIME declaration must not cause unsafe rendering.
- A Preview action may display the fallback message for unsupported or malformed content; a Download action remains available to an authorized user.
- PDF and image previews may use safe built-in browser facilities or a locally bundled viewer; no remote preview/conversion service is required.
- HTML, SVG, executable formats, office documents, archives, and other unlisted types are **not** supported preview formats in Sprint 3. They remain downloadable under Sprint 1 rules.
- The fallback message is user-visible UI text, not an SMTP error and not a reason to reject or modify an email.
- Sprint 3 does not introduce OCR, editing, attachment search, thumbnail generation, or additional message-delivery behavior.

## Regression boundary
All frozen Sprint 1 and Sprint 2 acceptance scenarios remain mandatory, including download-byte integrity, manager read-only access, and administrative dead-letter inspection. Sprint 3 adds independent preview scenarios in `4000-acceptance-criteria.md`.
