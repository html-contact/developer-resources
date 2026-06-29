---
title: HTML Forms
description: Use regular HTML form fields with your html.contact endpoint.
---

html.contact accepts normal browser form posts. You do not need a client SDK, a backend route, or a private API key for a website contact form.

## Public form keys

Public form keys start with `hc_pub_`.

They are safe to place in frontend HTML. A public key can submit to one form, but it cannot read submissions, change settings, send arbitrary email, or access account data.

The recommended form action uses the public key in the URL:

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
```

## Canonical form action

Use the `/f/{formKey}` endpoint for new forms:

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
```

Replace `hc_pub_REPLACE` with the public form key from your form.

## Minimum form

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required>

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required>

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <button type="submit">Send</button>
</form>
```

## Field names

Every successful input needs a `name` attribute. Most field names are stored as submitted. Use simple names such as `name`, `email`, `phone`, `subject`, and `message` when possible.

The submitted field named `subject` is stored as submitted data, can appear in the dashboard table, and is used as the notification email subject when present.

Unknown fields are preserved in submission data, API detail, and CSV exports. The dashboard table prioritizes common fields such as `name`, `email`, `subject`, `message`, and `phone`, so not every custom field appears as a table column. Duplicate field names are stored as multiple values, which is useful for checkbox groups.

## Fields and settings

Most forms should use normal submitted fields:

- `name`
- `email`
- `message`
- optional `subject`
- any custom field names your form needs

Normal fields are stored with the submission and can appear in email notifications, submission detail, API responses, and CSV exports. The dashboard table shows a few common fields first; it does not show every custom field at once.

## Notification subject and intro

The submitted field named `subject` is a normal submitted field. It is also used as the notification email subject when present.

Subject priority:

1. Submitted `subject` field.
2. Dashboard default subject.
3. `New submission from {form name}`.

The dashboard default intro is different: when a form has a default intro, html.contact always shows that intro at the top of notification emails. A submitted `_intro` field is stored as a normal field, but it does not override the dashboard intro.

## Reserved system fields

These names are read by html.contact and stored separately from normal submitted fields:

| Field | What html.contact does |
| --- | --- |
| `_redirect` | After a successful submission, this redirect is considered before the dashboard redirect setting. Redirects must use `https://`, except `http://localhost:*` and `http://127.0.0.1:*` for local development. |
| `_replyto` | Sets Reply-To from a literal email address or a submitted field name. For example, `_replyto=email` uses the submitted `email` field. |
| `_gotcha` | Honeypot field. Real visitors should leave it empty; filled values are rejected before email delivery. |
| `_hc_hp_*` | Honeypot field prefix. Any field name that starts with `_hc_hp_` is stored as a system field and rejected if filled. |
| `form_key` | Public form key used by the `/submit` compatibility endpoint. |
| `_form_name` | Reserved metadata field stored with system fields. It does not rename the form in your account. |
| `_source` | Reserved metadata field stored with system fields. It does not replace browser source checks. |
| `_tag` | Reserved metadata field stored with system fields. It does not route email. |
| `cf-turnstile-response` | Reserved for customer-managed challenge data. The default html.contact snippet does not require CAPTCHA or Turnstile code. |

If `_replyto` is absent, html.contact can still infer Reply-To from common submitted email fields such as `email`, `Email`, `your_email`, `your-email`, `reply_to`, `replyTo`, and `contact_email`.

## Fields that do not control behavior

These fields are allowed, but they are stored as ordinary submitted fields only:

- `_subject`
- `_intro`
- `_to`
- `_cc`
- `_bcc`
- `_from`

`_subject` does not override the notification subject. Use `subject` without the underscore.

`_intro` does not override the dashboard default intro.

`_to`, `_cc`, `_bcc`, and `_from` never control recipients, CC/BCC, sender identity, Reply-To, or routing. Recipients are configured inside html.contact or through the intended form-management API.

Private API keys that start with `hc_live_` should never appear in browser HTML.

## Success and redirects

After a successful browser post, html.contact can show its default success response or redirect the visitor. A submitted `_redirect` is considered first. If no `_redirect` is submitted, the form's default redirect setting is considered.

Redirect URLs must be absolute `https://` URLs. For local development, `http://localhost:*` and `http://127.0.0.1:*` are also allowed. If the chosen URL is missing or invalid, html.contact returns the normal success response instead of falling back to another redirect.

```html
<input type="hidden" name="_redirect" value="https://example.com/thanks">
```

## Compatibility endpoint

Use `/submit` only when you need the public key in the form body instead of the URL.

```html
<form action="https://html.contact/submit" method="POST">
  <input type="hidden" name="form_key" value="hc_pub_REPLACE">

  <input name="email" type="email" required>
  <textarea name="message" required></textarea>
  <button type="submit">Send</button>
</form>
```

Both `/f/hc_pub_REPLACE` and `/submit` use the same validation, spam screening, source checks, usage limits, and delivery flow.

For a complete field reference, see [Form Fields](/docs/form-fields/).

## JSON responses while testing

Public form submissions are form posts, not JSON API requests. If you want JSON back while testing with `curl`, send `Accept: application/json` or append `?format=json`.

```bash
curl "https://html.contact/f/hc_pub_REPLACE?format=json" \
  -H "Accept: application/json" \
  -H "Origin: https://example.com" \
  -H "Referer: https://example.com/contact" \
  --data-urlencode "email=mara@example.com" \
  --data-urlencode "message=Can you send pricing?"
```

Successful JSON response:

```json
{
  "ok": true,
  "submission_id": "sub_REPLACE",
  "email_status": "provider_accepted"
}
```

For failure codes and setup fixes, see [Error Responses](/docs/error-responses/) and [Troubleshooting](/docs/troubleshooting/).
