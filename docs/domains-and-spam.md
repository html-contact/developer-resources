---
title: Domains & Spam
description: Protect html.contact forms with allowed domains, spam screening, honeypots, quotas, and rate limits.
---

html.contact expects normal website forms to submit from trusted source domains. Source checks, spam screening, honeypots, quotas, and rate limits work together before a message reaches your inbox.

## Allowed domains

Add the exact hostnames that should be allowed to post to your form.

Examples:

- `example.com`
- `www.example.com`
- `contact.example.com`

If both `example.com` and `www.example.com` serve the form, add both. If you test from a preview URL, add that exact preview hostname while testing.

When a browser submits a form, html.contact checks `Origin` and `Referer` against the form's allowed domains. Submissions from other domains can be rejected before email delivery.

Source headers are useful abuse signals, but they are not cryptographic proof that a request came from a website. Keep server-side posts off for normal website forms.

## Server-side posts

Server-side posts allow trusted server or curl requests that may not include browser source headers.

Leave this setting off for normal website forms. Turning it on means website allowlisting can no longer prove that a post came from your site, so the submission relies more heavily on server-side spam checks, rate limits, and quotas.

## Server-side spam screening

html.contact screens obvious junk before delivery. The screening includes domain checks, automated spam screening, payload limits, rate limits, quotas, and honeypot checks.

Rejected submissions are separated from the main inbox so you can review them without polluting normal leads.

Submissions blocked as spam, or by an allowed-domain or honeypot check, keep only the first few kilobytes of their fields, and long request details such as the user agent or referring page are shortened too. Shortened values end with `…[truncated]`. These blocked submissions are deleted automatically 90 days after they arrive. Submissions you mark as spam yourself, and submissions held because your account reached its usage limit, stay until you delete them.

## Honeypot fields

You can add a visually hidden field for basic bot detection. Real visitors should not see it or fill it. Some simple bots fill every text input they find, so html.contact rejects the submission when the honeypot has a value.

A good honeypot is autofill-resistant: it looks like an ordinary text input to simple bots, but it does not look like a saved browser profile field.

```html
<div
  style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;"
  aria-hidden="true"
>
  <label for="hc-extra-field">Leave this field empty</label>
  <input
    id="hc-extra-field"
    name="_hc_hp_extra"
    type="text"
    tabindex="-1"
    autocomplete="new-password"
    data-1p-ignore="true"
    data-lpignore="true"
  >
</div>
```

Any field whose name starts with `_hc_hp_` is treated as a honeypot system field. `_gotcha` is also supported for older snippets.

Good neutral names:

- `_hc_hp_extra`
- `_hc_hp_optional`
- `_hc_hp_notes`
- `_hc_hp_details`
- `_hc_hp_blank`
- `_hc_hp_context`

Avoid profile or autofill-shaped names:

- `_hc_hp_company`
- `_hc_hp_name`
- `_hc_hp_email`
- `_hc_hp_phone`
- `_hc_hp_website`
- `_hc_hp_address`

Do not use words like `company`, `name`, `email`, `phone`, `website`, or `address` in the honeypot `name`, `id`, label, placeholder, or `autocomplete` value. Browsers and password managers can autofill offscreen fields when they look like real contact/profile fields, which can reject a real visitor as `honeypot_triggered`.

`autocomplete="new-password"`, `data-1p-ignore="true"`, and `data-lpignore="true"` help reduce accidental autofill. The most important protection is still choosing a neutral honeypot name. Do not make honeypot fields `required`. Prefer a visually hidden `type="text"` input over `type="hidden"` so simple bots can still find it.

## Rate limits

Public form submissions use source-based rate limits before rejected attempts are stored. Limits are designed to slow repeated automation without changing the normal browser form setup.

When a limit is hit, html.contact returns:

```json
{
  "ok": false,
  "error": {
    "code": "rate_limited",
    "message": "Too many submissions. Please try again shortly."
  }
}
```

## CAPTCHA

The default html.contact snippet does not require CAPTCHA or Turnstile code.

If you want a browser challenge, add your own CAPTCHA service on your site before posting to html.contact. Do not paste service-specific CAPTCHA fields into the default html.contact snippet unless your own site handles that service.
