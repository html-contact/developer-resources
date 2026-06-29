---
title: Troubleshooting
description: Fix common html.contact setup, testing, delivery, field, redirect, and attachment issues.
---

Start with the simplest check: confirm your form uses the right public endpoint and that every input you want to receive has a `name`.

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
```

Replace `hc_pub_REPLACE` with the public form key from your form.

## My deployed form says the domain is not allowed

`domain_not_allowed` means the browser source hostname did not match the form's allowed domains.

Fix:

- Add the exact hostname that serves the form.
- Add `example.com` and `www.example.com` separately if both serve it.
- Add preview hostnames separately while testing preview deployments.
- After changing domains, test from the deployed page again.

Browser source headers are useful abuse signals, but they are not proof that a request came from your site.

## curl or a server test says the origin is missing

`origin_missing` means the request did not include usable browser source headers and the form is not configured for direct server posts.

Normal website forms should be tested from the deployed page. For server-side scripts or curl tests, either include source headers while testing or enable **Allow API submissions** only when you intentionally need trusted server/curl posts.

```bash
curl "https://html.contact/f/hc_pub_REPLACE?format=json" \
  -H "Accept: application/json" \
  -H "Origin: https://example.com" \
  -H "Referer: https://example.com/contact" \
  --data-urlencode "email=mara@example.com" \
  --data-urlencode "message=Hello"
```

Leave direct server posts off for normal website forms.

## The form submits but no email arrives

The submission may still be stored even when email is not sent.

Check:

- The recipient email is verified.
- The recipient is not paused by a delivery failure.
- The account is not at its submission usage limit.
- The submission is not in rejected or spam results.
- The email did not land in spam or filtering on the receiving mailbox.

In html.contact, open the submission and check the email status and request checks.

## My custom field is missing

Every field you want to receive needs a `name` attribute.

```html
<input id="company" name="company">
```

The `id` connects labels and browser behavior. The `name` is what gets submitted and stored.

For selects, radios, and checkboxes, html.contact stores the submitted `value`.

```html
<select id="budget" name="budget">
  <option value="under-5k">Under $5k</option>
</select>
```

The stored value is `under-5k`.

## My subject or intro is not what I expected

Use `subject` without an underscore when the submitted form should set the notification subject.

Subject priority:

1. Submitted `subject`.
2. The form's default subject setting.
3. `New submission from {form name}`.

The form's default intro is always shown when configured. A submitted `_intro` field is stored as a normal submitted field, but it does not override the default intro.

`_subject` does not set the notification subject.

## Replies do not go to the submitter

Add `_replyto=email` when your form has an `email` field and you want replies to go to the submitted email address.

```html
<input type="hidden" name="_replyto" value="email">
```

If `_replyto` is absent, html.contact can infer Reply-To from common email fields such as `email`, `Email`, `your_email`, `your-email`, `reply_to`, `replyTo`, and `contact_email`.

## Redirect did not happen

Redirect priority is:

1. Submitted `_redirect`.
2. The form's **Redirect URL** setting.
3. Normal html.contact success response.

If the chosen redirect URL is missing or invalid, html.contact shows the normal success response instead of falling back to another redirect.

Use a full `https://` URL. For local development, `http://localhost:*` and `http://127.0.0.1:*` are also allowed.

```html
<input type="hidden" name="_redirect" value="https://example.com/thanks">
```

## Attachment upload failed

Check:

- The form has `enctype="multipart/form-data"`.
- The file input has a `name`.
- Only one non-empty attachment is selected.
- The attachment is 4 MB or smaller.
- The whole submission payload is not too large.

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

Email notifications link to authenticated attachment downloads. In the dashboard, open the submission, find **Attachments**, then use **File** to view or download the upload.

Treat unknown uploads like any other unknown attachment: scan downloads with your security tools before opening them.

## Hidden routing fields are not working

Public form fields do not control recipients, CC/BCC, sender identity, or routing.

These fields are stored as ordinary submitted fields if you use them, but they never route email:

- `_to`
- `_cc`
- `_bcc`
- `_from`

Configure recipients inside html.contact.

## The form is paused

`form_inactive` means the form is not accepting submissions. Resume the form in html.contact before testing again.

## I hit a rate or usage limit

`rate_limited` means too many submissions arrived in the current rate-limit window. Wait and retry later.

`usage_limit_reached` means the account reached its submission usage limit. Review usage in html.contact before testing again.

## I need the exact error code

For browser forms, html.contact can show an HTML error page. For JSON while testing, send `Accept: application/json` or add `?format=json`.

See [Error Responses](/docs/error-responses/) for the public form error table.
