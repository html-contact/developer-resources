---
title: Agency API Overview
description: Automate Agency accounts, clients, forms, submissions, exports, and attachments with account-owned API keys.
---

The `/api/v1` developer API is an Agency API for trusted server-side automation. It is available to Agency pilot accounts, which are enabled on request; it is not part of Free, Starter, or Pro. Browser forms still submit with public `hc_pub_` keys; see [HTML Forms](html-forms.md).

## Authentication and tenant paths

Create a Full or Restricted API key in **Agency settings**. The secret begins with `hc_live_`, is shown once, and must stay out of browser code, prompts, logs, and repositories. Choose no expiration for durable integrations or an optional expiry up to one year away.

```http
Authorization: Bearer hc_live_REPLACE
```

Every private path names the credential's account and, where applicable, its client:

```txt
https://html.contact/api/v1/accounts/{accountId}/clients/{clientId}
```

An account-owned credential has an immutable client policy: selected clients (`fixed`) or all present and future clients (`all_clients`). Capabilities further reduce what it can do. A legacy personal `hc_live_` key is not an Agency credential and cannot authorize these routes.

## Endpoint groups

| Group | Operations |
| --- | --- |
| Account | Read account identity; read account-wide usage with an all-client credential. |
| Clients | List, create, read, rename, archive, and restore clients. Client creation requires all-client authority. |
| Forms | List, create a recipient-free draft, read, update bounded non-routing settings, activate, and pause. |
| Submissions | List content-free client/form metadata, read bounded detail, and export up to 5,000 filtered rows as CSV. |
| Attachments | Download one private attachment; safe media types may request `disposition=inline`. |

Recipient management, test email, form duplication/deletion, submission mutation, billing management, API credential management, integrations, and `/api/app` dashboard routes are not part of this contract.

## Safe mutation protocol

Every create or update request needs a unique `Idempotency-Key`. Updates, archive/restore, activate, and pause also need the latest quoted `ETag` value in `If-Match`.

```http
Idempotency-Key: 018f-example-unique-request
If-Match: "revision-token-from-latest-read"
```

Reuse the same idempotency key only to retry the identical request. A completed retry returns `Idempotency-Replayed: true`. Read the resource again after `412 precondition_failed`; a missing precondition returns `428`. Turning on email notifications for an active form, or activating a form with notifications on, needs a verified, deliverable To recipient. Otherwise the request returns `409 recipient_not_ready`: fix the recipient in the dashboard, because retrying the same request will not help.

## Pagination, filtering, and limits

Lists use `limit` and an opaque, filter-bound `cursor`; follow `nextCursor` without inspecting or changing it. Submission lists support `dateFrom`, `dateTo`, `status`, and `spamResult`. CSV adds `formId`, is capped at 5,000 rows, and carries at most 100 field columns: when submissions use more distinct field names, the most common names keep their own column and the remaining fields are folded into one `Other Fields` column as JSON.

List responses expose content-free submission metadata. Detail requires the stronger `submissions:read` capability and returns bounded visitor fields plus safe attachment metadata. Treat all visitor values as untrusted personal data.

Requests have separate read, mutation, export, and attachment-download rate limits. A `429` response may include `Retry-After`. Every response includes `X-Request-Id` for support and correlation.

## Response shape

```json
{ "ok": true, "data": { "id": "client_REPLACE", "revision": 1 } }
```

```json
{
  "ok": false,
  "error": {
    "code": "forbidden",
    "message": "This credential does not have the required capability.",
    "requestId": "request_REPLACE"
  }
}
```

Use [Authentication](api/authentication.md) for credential rules, [Examples](api/examples.md) for copyable requests, and the generated [OpenAPI document](https://html.contact/openapi.json) for exact schemas and `x-required-capabilities`.
