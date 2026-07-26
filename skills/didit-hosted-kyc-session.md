---
name: Run a hosted KYC verification session
description: Create a Didit hosted verification session, send the user to it, then read the decision.
api: openapi/didit-openapi-original.json
operations: [post_v3_session_create, get_v3_session_decision, create_webhook_destination]
auth: x-api-key header
---

# Run a hosted KYC verification session

Use Didit's hosted flow to verify an end user without building UI.

## Steps
1. **Create the session** — `POST /v3/session/` (`post_v3_session_create`) with a `workflow_id`
   and your `vendor_data` reference. The response returns a hosted `url` and a `session_token`.
2. **Send the user** to the returned hosted `url`. They complete ID capture + liveness in the
   browser/WebView.
3. **Get the decision** — poll `GET /v3/session/{sessionId}/decision/` (`get_v3_session_decision`)
   for the full decision payload (plural feature arrays: `id_verifications[]`, `liveness_checks[]`,
   `face_matches[]`, `aml_screenings[]`), or, preferred, receive it via webhook.
4. **Prefer webhooks over polling** — register once with `POST /v3/webhook/destinations/`
   (`create_webhook_destination`) subscribing to `status.updated`; verify the `X-Signature-V2`
   HMAC-SHA256 header and reject if `abs(now - X-Timestamp) > 300s`.

## Rules
- Auth: `x-api-key` header (see authentication/didit-authentication.yml).
- No request idempotency key; webhook `event_id` is the consumer-side dedup key.
- Pagination on list endpoints is `limit`/`offset`. Errors are plain HTTP status + JSON body.
