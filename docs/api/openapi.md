---
title: OpenAPI
description: Use the html.contact OpenAPI document for schemas, examples, response shapes, and API tooling.
---

The generated OpenAPI document is the source of truth for html.contact's public API contract.

It includes:

- Public `hc_pub_` form submission routes.
- Private `hc_live_` Bearer API routes.
- Request bodies and query parameters.
- Response schemas and examples.
- Error response shapes.
- Required API key scopes through `x-required-scopes`.

It does not include account-area routes for billing or payment settings, API-key management, linked-email management, profile settings, form deletion, bulk submission mutation, auth helper routes, or unimplemented agent routes. It may include the safe submission usage summary.

## Links

<p>
  <a href="/openapi.json" target="_blank" rel="noreferrer">Open raw OpenAPI JSON</a>
  <br>
  <a href="https://petstore.swagger.io/?url=https://html.contact/openapi.json" target="_blank" rel="noreferrer">View in Swagger UI</a>
</p>

## Security scheme

Private endpoints use Bearer auth:

```http
Authorization: Bearer hc_live_REPLACE
```

Public form endpoints do not use Bearer auth. They use a public `hc_pub_` form key.

## Endpoint groups

| Tag | Included routes |
| --- | --- |
| Public Forms | `/f/{formKey}`, `/submit` |
| Forms | `/api/v1/forms`, `/api/v1/forms/{formId}`, `/api/v1/forms/{formId}/duplicate`, `/api/v1/forms/{formId}/test-email`, `/api/v1/forms/{formId}/verify-recipient` |
| Submissions | Submission lists, CSV exports, and submission detail |
| Attachments | Authenticated attachment download and inline preview |
| Usage | Safe submission usage summary |

## Response shape

Successful private JSON responses include `ok: true` and a route-specific payload.

Errors use:

```json
{
  "ok": false,
  "error": {
    "code": "domain_not_allowed",
    "message": "This form is not allowed on this website."
  }
}
```

## Important schema notes

Submission list and detail responses include JSON-string response fields:

- `fields_json`
- `fields_order_json`
- `system_fields_json`
- `attachments_json`
- `events_json` on submission detail responses

Parse those client-side when you need structured field, attachment, or detail event data.

The `subject` field is stored as submitted data and can control the notification email subject. `_subject` and `_intro` are ordinary submitted fields. Reserved system fields are stored in `system_fields_json`, not in normal submitted-field columns.

## Attachments

Attachment download responses return the stored attachment content type when available, with `application/octet-stream` as a fallback. Safe previewable file types may be opened with `?disposition=inline`; inline previews are sandboxed.

## Versioning

The OpenAPI document reports version `0.1.0`. Compatibility-sensitive changes should be made in the product behavior and OpenAPI document together.
