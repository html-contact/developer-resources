---
title: API Overview
description: Use html.contact private API keys for form management, submissions, exports, attachments, and usage summaries.
---

This page covers the private API for trusted servers, automations, and approved agents.

For browser forms, start with [HTML Forms](/docs/html-forms/). Public `hc_pub_` keys belong in frontend HTML and only submit forms.

Private API keys start with `hc_live_` and are sent as Bearer tokens. Keep them out of frontend code.

## Create form vs submit form

- Create form: `POST /api/v1/forms` creates a form configuration and public `hc_pub_` endpoint. It requires a trusted `hc_live_` Form manager key.
- Submit form: `POST /f/{formKey}` or `POST /submit` stores a visitor's submission. It uses a public `hc_pub_` form key and never creates forms, accounts, or API keys.

## Base URL

```txt
https://html.contact
```

## Key types

| Prefix | Where it belongs | Purpose |
| --- | --- | --- |
| `hc_pub_` | Frontend HTML | Public form key used by browser forms. See [HTML Forms](/docs/html-forms/). |
| `hc_live_` | Trusted server, automation, or approved agent | Private API key used with `Authorization: Bearer`. Never expose in browser code. |

## Private API endpoints

Private endpoints require:

```txt
Authorization: Bearer hc_live_REPLACE
```

| Method | Path | Purpose |
| --- | --- | --- |
| `GET` | `/api/v1/forms` | List forms. |
| `POST` | `/api/v1/forms` | Create a form. |
| `GET` | `/api/v1/forms/{formId}` | Get one form. |
| `PATCH` | `/api/v1/forms/{formId}` | Update form settings. |
| `POST` | `/api/v1/forms/{formId}/duplicate` | Duplicate a form. |
| `POST` | `/api/v1/forms/{formId}/test-email` | Send a test notification. |
| `POST` | `/api/v1/forms/{formId}/verify-recipient` | Send or check recipient verification. |
| `GET` | `/api/v1/forms/{formId}/submissions` | List submissions for one form. |
| `GET` | `/api/v1/forms/all/submissions` | List submissions across all forms. |
| `GET` | `/api/v1/forms/{formId}/submissions.csv` | Export one form's submissions as CSV. |
| `GET` | `/api/v1/forms/all/submissions.csv` | Export workspace submissions as CSV. |
| `GET` | `/api/v1/submissions/{submissionId}` | Get submission detail. |
| `GET` | `/api/v1/submissions/{submissionId}/attachments/{attachmentId}` | Download or preview a private attachment. |
| `GET` | `/api/v1/usage` | Read a safe submission usage summary. |

## API key presets

html.contact API keys use scoped presets.

| Access level | Scopes | Can access |
| --- | --- | --- |
| Read only | `forms:read`, `submissions:read` | List/get forms, read submissions, export CSV, download authenticated attachments, and read usage summary. |
| Form manager | `forms:create`, `forms:read`, `forms:update`, `submissions:read` | Everything read-only can do, plus create, update, and duplicate forms, send test email, and send/check recipient verification. |

API keys cannot manage account settings, billing or payment settings, API keys, linked emails, profile settings, form deletion, bulk submission mutation, auth helper routes, or unimplemented agent routes.

Billing, payment methods, invoices, cancellation, and plan changes are managed outside the private API.

## Source protection

For normal website forms, html.contact checks `Origin` and `Referer` against the form's allowed domains. This helps block casual misuse, but request headers are not cryptographic proof that a request came from a website.

If a form enables server-side posts, server or curl requests without browser source headers may be accepted. Use that mode only for trusted server-side workflows.

## Submitted fields

Every input you want to store must have a `name` attribute. Unknown fields are preserved in submission data, API detail, and CSV exports. Duplicate field names are stored as arrays.

The field named `subject` is stored as submitted data, can appear in dashboard table columns, and controls the notification email subject when present. When no submitted `subject` exists, html.contact uses the form's default subject setting, then falls back to the form name. The dashboard default intro is the notification intro when configured. `_subject` and `_intro` are ordinary submitted fields and do not override notification subject or intro behavior.

Fields such as `_to`, `_cc`, `_bcc`, and `_from` are preserved as submitted data but never control email routing. Routing is configured from verified recipient settings in your account or through the form-management API.

Reserved system fields such as `_redirect`, `_replyto`, `_form_name`, `_source`, `_tag`, `_gotcha`, `_hc_hp_*`, `cf-turnstile-response`, and `form_key` are stored separately in `system_fields_json`; they are not normal submitted-field columns. `_form_name`, `_source`, and `_tag` are metadata-only system fields today.

## Form response shape

Form-management responses return camelCase JSON DTOs. The database stores some settings internally as snake_case integer flags, but the private API returns parsed domains and true JSON booleans.

```json
{
  "ok": true,
  "form": {
    "id": "form_REPLACE",
    "publicKey": "hc_pub_REPLACE",
    "name": "Website contact",
    "status": "active",
    "recipientEmail": "owner@example.com",
    "recipientVerifiedAt": "2026-06-15T18:04:12.000Z",
    "allowedDomains": ["example.com", "www.example.com"],
    "defaultSubject": "New website inquiry",
    "defaultIntro": "A new message arrived from your website.",
    "defaultRedirectUrl": "https://example.com/thanks",
    "emailNotificationsEnabled": true,
    "allowDirectPosts": false,
    "honeypotField": "_gotcha",
    "lifetimeSubmissionCount": 7,
    "lifetimeSubmissionLimit": 250,
    "createdAt": "2026-06-15T17:50:30.000Z",
    "updatedAt": "2026-06-15T18:04:12.000Z",
    "formRecipients": [
      {
        "email": "owner@example.com",
        "role": "to",
        "verificationStatus": "verified",
        "deliverabilityStatus": "ok",
        "verifiedAt": "2026-06-15T18:04:12.000Z"
      }
    ],
    "snippet": "<form ...></form>"
  }
}
```

## Form notification settings

Form create and update requests can include `emailNotificationsEnabled` as a JSON boolean. When omitted on create, notifications default to enabled.

```json
{
  "emailNotificationsEnabled": false
}
```

Set it to `false` to store accepted submissions without sending notification emails. Set it to `true` to resume notification email attempts. Response objects return `emailNotificationsEnabled` as a JSON boolean.

Reply-To behavior is controlled by public submission fields such as `_replyto` and common fields like `email`. `defaultReplyToField` is not part of the private API contract and is rejected by `/api/v1` form updates.

## Submissions

Submission list endpoints support:

- `folder=inbox|spam|archived`
- `q`
- `limit`
- `cursor`
- `status`
- `email_status`
- `source_host`
- `date_from`
- `date_to`

Submission list responses include submitted fields and attachment metadata. Submission detail responses include the same core submission fields plus event data. Values marked as JSON strings should be parsed client-side.

The signed-in dashboard table shows up to five dynamic submitted-field columns and prioritizes `name`, `email`, `subject`, `message`, and `phone`. CSV exports include all exported submitted-field columns.

## Usage boundary

Billing, payment methods, invoices, cancellation, and plan changes are managed outside the private API.

A safe usage summary is available through the API so integrations can warn before an account reaches its submission limit.

The private API and OpenAPI document stay focused on form creation and configuration, submission JSON, CSV exports, attachment access, and safe usage summaries.

Public submissions can still return `usage_limit_reached` when an account is over its server-owned cap.

## Attachments

Forms may include one file input. Files are stored privately and require authenticated access to download.

CSV exports include attachment metadata and authenticated download links. CSV exports do not include file bytes, internal storage identifiers, or unauthenticated file URLs.

## OpenAPI

Use the generated OpenAPI document for machine-readable schemas, request bodies, examples, and response shapes.

<p>
  <a href="/openapi.json" target="_blank" rel="noreferrer">Open raw OpenAPI JSON</a>
  <br>
  <a href="https://petstore.swagger.io/?url=https://html.contact/openapi.json" target="_blank" rel="noreferrer">View in Swagger UI</a>
</p>

See also:

- [Authentication](/docs/api/authentication/)
- [Examples](/docs/api/examples/)
- [OpenAPI](/docs/api/openapi/)
