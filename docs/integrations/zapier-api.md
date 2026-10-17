---
title: Zapier connector API
description: OAuth-only API contract for the native html.contact Zapier app.
---

This API is for the native html.contact Zapier app, which is available to invited pilot accounts. It is separate from `/api/v1` and dashboard integration management. See the [Zapier setup guide](zapier.md) for customer instructions.

The connector exposes only forms in an account's original **My forms** workspace. Agency client resources are denied.

Base/resource: `https://html.contact/api/integrations/zapier/v1`.

Only OAuth access tokens issued to the controlled confidential Zapier client with this exact audience are accepted. Dashboard cookies, MCP tokens and `hc_live_` keys are rejected. Every request checks the live account grant; form operations additionally require explicit selected-form membership and current form eligibility. Scopes are `offline_access`, `zapier:forms:read`, `zapier:submissions:read`, `zapier:attachments:read`, and `zapier:subscriptions:write`.

| Method and path | Request | Response |
| --- | --- | --- |
| `GET /connection` | Bearer token | `{ok:true,id,email,label}`; label is the account email |
| `GET /forms?after=ID` | Optional last form ID, at most 128 characters | `{ok:true,forms:[{id,name}],next_cursor}`; 50 per page, ascending ID |
| `GET /forms/:formId/submissions` | Approved form | `{ok:true,submissions:[...]}`; up to three real accepted records ordered newest timestamp, then descending ID |
| `POST /subscriptions` | JSON `{form_id,target_url}` | `{ok:true,id}`; repeating an active account/form/target request returns the same ID |
| `DELETE /subscriptions/:id` | Bearer token | 204; owner-scoped and idempotent |
| `POST /deliveries/:id/resolve` | JSON `{subscription_id,proof}` | `{ok:true,submission:{...}}`; only retained, eligible, authorized content |
| `GET /submissions/:id/attachments/:attachmentId` | Bearer token | Private binary stream with attachment disposition, content type and length |

JSON mutation bodies are limited to 16 KiB. API requests have an account budget of 300 per minute; 429 responses include Retry-After. JSON errors use `{ok:false,error:{code,message}}`; 401 means refresh or reconnect, 404 means unavailable or unauthorized record, and 409 means an access revision or subscription-capacity conflict. Responses are non-cacheable. No record-existence distinction is disclosed across account boundaries.

Subscription targets must be canonical HTTPS URLs on exactly `hooks.zapier.com`, without credentials, an explicit port, fragments or redirects. Complete URLs are encrypted at rest. Separate subscriptions use separate returned IDs; an old unsubscribe never removes a replacement. Capacity is ten active Zapier subscriptions per form.

The notification is `{type:"submission.accepted",delivery_id,proof}`. The purpose-bound encrypted proof identifies the delivery and subscription and expires with seven-day retention. It contains no submitted fields. The connector authenticates to resolve it, and the server checks grant generation, selected form, source status, subscription and retained payload before returning the frozen projection. Resolution can be repeated while authorized: it is not a consume-before-return deduplication mechanism.

Projection: `id`, `form_id`, `form_name`, `created_at`, `dashboard_url`, `fields` (original string or string-array values), and `attachment` (null or safe ID/name/type/size/field metadata). Internal object keys and network/security metadata are excluded. The connector maps original names to injective `field_` + UTF-8 hex keys, labels them with the original name, preserves original fields as JSON, and uses the identical serializer for live events and samples. Its file dehydration arguments contain submission and attachment IDs only.

Transport is at least once. A stable ID does not prevent duplicate instant-trigger Zap runs. Existing retry/backoff, queue reconciliation, history and encryption infrastructure is reused. Operational flags pause dispatch without cancelling eligible jobs; customer removal revokes access and cancels dependent jobs. HTTP 410 retires the affected subscription. Downloaded copies cannot be recalled after revocation.
