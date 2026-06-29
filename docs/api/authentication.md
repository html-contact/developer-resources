---
title: Authentication
description: Understand html.contact private API keys, scopes, and authorization errors.
---

html.contact private API calls use `hc_live_` keys. Public browser form keys start with `hc_pub_`; see [HTML Forms](/docs/html-forms/) for public-key form setup.

## Private API keys

Private API keys start with `hc_live_`.

Send them as Bearer tokens:

```http
Authorization: Bearer hc_live_REPLACE
```

Keep private API keys on trusted servers, scripts, backend jobs, or approved agents. Never put a private key in browser JavaScript, static HTML, public GitHub repositories, or client-side environment variables.

## API key presets

API keys are scoped. Use the smallest preset that fits the job.

| Preset | Scopes | Intended use |
| --- | --- | --- |
| Read only | `forms:read`, `submissions:read` | Reporting, exports, trusted reporting tools, and read-only agents. |
| Form manager | `forms:create`, `forms:read`, `forms:update`, `submissions:read` | Trusted setup agents or server workflows that create and configure forms. |

## What API keys cannot do

Generated API keys cannot:

- Manage account settings.
- Manage billing or payment settings.
- Create or revoke API keys.
- Manage linked emails.
- Manage profile settings.
- Delete forms.
- Mutate submissions in bulk.
- Access auth helper routes or unimplemented agent routes.

Manage API keys and linked emails from your signed-in account area.

## Form Manager permissions

Form Manager keys can:

- List forms.
- Create forms.
- Update form settings.
- Duplicate forms.
- Send test emails.
- Send or check recipient verification.
- List submissions and export CSV.
- Download authenticated submission attachments.

They cannot bypass linked-email verification. Recipients, CC, and BCC still need to be verified linked emails.

Billing and payment settings are not part of the private API contract. API keys may read a safe submission usage summary for limit warnings; manage billing in the signed-in account area.

## Error shape

Errors use this shape:

```json
{
  "ok": false,
  "error": {
    "code": "forbidden",
    "message": "Authentication required."
  }
}
```

Common private API errors include:

| Code | Meaning |
| --- | --- |
| `unauthorized` | Authentication is missing on routes that allow an unauthenticated check. |
| `forbidden` | The key is missing, invalid, expired, or does not have the required scope. |
| `form_not_found` | The form does not exist or does not belong to the authenticated actor. |
| `submission_not_found` | The submission does not exist or does not belong to the authenticated actor. |
| `recipient_not_verified` | The selected linked email is not verified. |
| `recipient_suppressed` | The recipient is suppressed after a delivery failure. |
| `rate_limited` | The action is temporarily rate-limited. |

Private API routes commonly return `forbidden` when the key is missing, invalid, expired, or does not have the required scope.

## OpenAPI

<p>
  <a href="/openapi.json" target="_blank" rel="noreferrer">Open raw OpenAPI JSON</a>
  <br>
  <a href="https://petstore.swagger.io/?url=https://html.contact/openapi.json" target="_blank" rel="noreferrer">View in Swagger UI</a>
</p>
