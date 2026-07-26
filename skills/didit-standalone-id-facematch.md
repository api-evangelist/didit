---
name: Standalone ID verification + face match
description: Verify a document and match it to a selfie server-to-server, no hosted UI.
api: openapi/didit-openapi-original.json
operations: [post_v3id-verification, post_v3passive-liveness, post_v3face-match]
auth: x-api-key header
---

# Standalone ID verification + face match

Backend-to-backend identity check when you already collected the images.

## Steps
1. **Verify the document** — `POST /v3/id-verification/` (`post_v3id-verification`) as
   `multipart/form-data` with the document image(s). Returns OCR fields and an authenticity result.
2. **Check liveness** — `POST /v3/passive-liveness/` (`post_v3passive-liveness`) on the selfie
   to confirm a live person (not a replay/print).
3. **Match faces** — `POST /v3/face-match/` (`post_v3face-match`) with the document portrait and
   the selfie for a 1:1 similarity score.

## Rules
- Auth: `x-api-key` header. Uploads are `multipart/form-data`; JSON bodies are `application/json`.
- Respect rate limits (free 10 rpm / paid 600 rpm; 429 on exceed) — see conventions/didit-conventions.yml.
- For sanctions/PEP screening add `POST /v3/aml/` (`post_v3aml`).
