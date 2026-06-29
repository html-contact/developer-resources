---
title: Error Responses
description: Public form submission error codes, what they mean, and how to fix them.
---

Normal browser form posts receive an HTML success or error page. When you test with `curl` or a server-side script, request JSON with `Accept: application/json` or `?format=json`.

```bash
curl "https://html.contact/f/hc_pub_REPLACE?format=json" \
  -H "Accept: application/json" \
  -H "Origin: https://example.com" \
  -H "Referer: https://example.com/contact" \
  --data-urlencode "email=mara@example.com" \
  --data-urlencode "message=Hello"
```

JSON errors use this shape:

```json
{
  "ok": false,
  "error": {
    "code": "domain_not_allowed",
    "message": "This form is not allowed on this website."
  }
}
```

## Public form errors

| Status | Code | What it means | How to fix |
| --- | --- | --- | --- |
| `400` | `validation_error` | The form payload could not be parsed, a field name was too long, or spam screening rejected a link-heavy message as invalid. | Check the form encoding and field names. Keep field names short and avoid spammy/link-heavy content. |
| `400` | `honeypot_triggered` | A honeypot field was filled. | Keep `_gotcha` and any `_hc_hp_*` fields empty for real visitors. Make sure custom scripts and autofill are not populating honeypot fields. |
| `400` | `spam_detected` | Server-side spam screening rejected the message. | Review the submitted content, field names, and automation behavior. |
| `400` | `too_many_fields` | The submission included too many fields. | Remove unnecessary inputs before submitting. |
| `400` | `too_many_files` | More than one non-empty attachment was submitted. | Use one file input with one selected file. |
| `400` | `file_too_large` | An attachment exceeded 4 MB. | Upload a smaller file. |
| `400` | `payload_too_large` | The submitted text or attachment payload was too large. | Reduce field content or attachment size. |
| `402` | `usage_limit_reached` | The account has reached its submission usage limit. | Review usage in html.contact and upgrade or wait for the next period when applicable. |
| `403` | `origin_missing` | No usable browser source header was present, and direct server posts are not enabled for the form. | Test from the deployed page, include browser-like source headers in server tests, or enable API submissions only when you intentionally need trusted server/curl posts. |
| `403` | `domain_not_allowed` | The browser source hostname did not match the form's allowed domains. | Add the exact hostname that serves the form, such as `example.com` and `www.example.com` separately. |
| `403` | `form_inactive` | The form is paused or otherwise not accepting submissions. | Resume the form in html.contact. |
| `404` | `form_not_found` | The public form key is missing or invalid. | Replace the placeholder with the real `hc_pub_` key from your form. For `/submit`, include `form_key`. |
| `429` | `rate_limited` | Too many submissions arrived in the current rate-limit window. | Wait and retry later. If this happens during testing, slow down repeated requests. |
| `500` | `attachment_store_failed` | The submission was accepted, but the uploaded attachment could not be stored. | Retry the submission. If it keeps happening, remove the attachment and contact support. |

## Success responses

For JSON tests, a successful submission returns:

```json
{
  "ok": true,
  "submission_id": "sub_REPLACE",
  "email_status": "provider_accepted"
}
```

`email_status` may also be `not_attempted` when the submission was stored but email delivery was not attempted, such as when the recipient is not ready for delivery.

## Redirect responses

A successful browser post can return a `303` redirect. html.contact considers submitted `_redirect` first, then the form's **Redirect URL** setting.

Redirect URLs must be absolute `https://` URLs. For local development, `http://localhost:*` and `http://127.0.0.1:*` are also allowed. If the chosen redirect URL is missing or invalid, html.contact returns the normal success response instead of redirecting.

## Private API errors

This page focuses on public form submissions. Private API routes use the same general JSON error shape, but private API authentication, scopes, and endpoint-specific responses are documented in the [Private API docs](/docs/api/).
