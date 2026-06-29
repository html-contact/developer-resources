---
title: Form Fields
description: Understand normal fields, reserved system fields, reply-to, redirects, subjects, and custom field storage.
---

html.contact reads normal HTML form fields. Every field you want to collect needs a `name` attribute.

## Normal submitted fields

Most forms should use ordinary field names:

- `name`
- `email`
- `message`
- optional `subject`
- any custom field names your form needs

html.contact stores submitted values by field name. For text fields, the stored value is what the visitor typed. For selects, radios, and checkboxes, the stored value is the submitted `value`.

```html
<label for="budget">Budget</label>
<select id="budget" name="budget">
  <option value="under-5k">Under $5k</option>
  <option value="5k-15k">$5k - $15k</option>
  <option value="15k-plus">$15k+</option>
</select>
```

If the visitor chooses "Under $5k", html.contact stores `budget` with the value `under-5k`.

## Custom fields

Unknown field names are preserved in submitted data, submission detail, private API responses, and CSV exports.

The dashboard table shows a small set of common fields first, such as `name`, `email`, `subject`, `message`, and `phone`. It does not show every custom field as a table column at once.

Duplicate field names are stored as multiple values. This is useful for checkbox groups:

```html
<label>
  <input type="checkbox" name="services" value="Design">
  Design
</label>

<label>
  <input type="checkbox" name="services" value="Development">
  Development
</label>
```

If both are selected, both values are preserved.

## Subject and intro

Use `subject` without an underscore when the submitted form should set the notification email subject.

Subject priority:

1. Submitted `subject` field.
2. The form's default subject setting.
3. `New submission from {form name}`.

The form's default intro is different. When configured, it is always shown at the top of notification emails for that form. A submitted `_intro` field is stored as a normal submitted field, but it does not override the default intro.

## Reply-To

Use `_replyto` when you want replies to go to the person who submitted the form.

```html
<input type="hidden" name="_replyto" value="email">
```

`_replyto` can be:

- a literal email address, such as `support@example.com`
- the name of a submitted field, such as `email`

If `_replyto` is absent, html.contact can infer Reply-To from common submitted email field names such as `email`, `Email`, `your_email`, `your-email`, `reply_to`, `replyTo`, and `contact_email`.

## Redirects

After a successful browser post, html.contact can show its normal success response or redirect the visitor.

Redirect priority:

1. Submitted `_redirect`.
2. The form's **Redirect URL** setting.
3. Normal html.contact success response.

```html
<input type="hidden" name="_redirect" value="https://example.com/thanks">
```

Redirect URLs must be absolute `https://` URLs. For local development, `http://localhost:*` and `http://127.0.0.1:*` are also allowed. If the chosen redirect URL is missing or invalid, html.contact shows the normal success response instead of falling back to another redirect.

## Reserved system fields

These field names are read by html.contact and stored separately from normal submitted fields:

| Field | What html.contact does |
| --- | --- |
| `_redirect` | Considers this URL for post-submit redirect before the form's Redirect URL setting. |
| `_replyto` | Sets Reply-To from a literal email address or from another submitted field. |
| `_gotcha` | Honeypot field. Filled values are rejected before email delivery. |
| `_hc_hp_*` | Honeypot field prefix. Any field name beginning with `_hc_hp_` is stored separately and rejected if filled. |
| `form_key` | Public form key used by the `/submit` compatibility endpoint. |
| `_form_name` | Reserved metadata field. It does not rename the form in your account. |
| `_source` | Reserved metadata field. It does not replace browser source checks. |
| `_tag` | Reserved metadata field. It does not route email. |
| `cf-turnstile-response` | Reserved for customer-managed challenge data. Default snippets do not require CAPTCHA or Turnstile. |

## Fields that do not control email

These fields are allowed, but they are stored as ordinary submitted fields only:

- `_subject`
- `_intro`
- `_to`
- `_cc`
- `_bcc`
- `_from`

`_subject` does not set the notification subject. Use `subject` without the underscore.

`_intro` does not override the form's default intro.

`_to`, `_cc`, `_bcc`, and `_from` never control recipients, CC/BCC, sender identity, Reply-To, or routing. Recipients are configured inside html.contact.

## File fields

File inputs require `enctype="multipart/form-data"`.

```html
<form
  action="https://html.contact/f/hc_pub_REPLACE"
  method="POST"
  enctype="multipart/form-data"
>
  <input id="attachment" name="attachment" type="file">
  <button type="submit">Send</button>
</form>
```

The file input still needs a `name`. The uploaded filename is stored with the submitted fields under that field name.

Current browser form support allows one non-empty attachment per submission, up to 4 MB.
