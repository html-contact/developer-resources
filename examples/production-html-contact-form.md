---
title: Production HTML Contact Form
description: Add reply-to, honeypot support, subject, redirect settings, and optional attachment handling to a plain HTML form.
---

Start with a normal form post, then add the small fields most production contact forms use.

Replace `hc_pub_REPLACE` with the public form key from your form.

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
  <input type="hidden" name="_replyto" value="email">

  <div
    style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;"
    aria-hidden="true"
  >
    <label for="hc-company">Company</label>
    <input
      id="hc-company"
      name="_hc_hp_company"
      type="text"
      tabindex="-1"
      autocomplete="off"
    >
  </div>

  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required>

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required>

  <label for="subject">Subject</label>
  <input id="subject" name="subject" value="Website inquiry">

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <button type="submit">Send</button>
</form>
```

## What the extra fields do

| Field | Behavior |
| --- | --- |
| `_replyto` | Sets reply-to from a literal email address or a submitted field name. `email` means use the submitted `email` field. If omitted, html.contact can infer common email fields such as `email`, `Email`, `your_email`, `your-email`, `reply_to`, `replyTo`, and `contact_email`. |
| `_hc_hp_*` | Honeypot field prefix. Real visitors leave these visually hidden fields empty; filled values are rejected before email delivery. `_gotcha` is still supported for older snippets. |
| `subject` | Normal submitted field that can appear in submissions and set the notification subject. If no `subject` field is submitted, html.contact uses the dashboard default subject. |

If your form has a dashboard default intro, html.contact always shows that intro at the top of notification emails. A submitted `_intro` field is stored as a normal field and does not override the dashboard intro.

## Optional thank-you redirect

You can set a default redirect in the form settings under **Redirect URL**. That setting sends visitors to your thank-you page after a successful browser post.

You can also submit `_redirect` from the form when this specific page should use a different thank-you page.

```html
<input type="hidden" name="_redirect" value="https://example.com/thanks">
```

Redirect priority matches the app settings:

1. Submitted `_redirect`.
2. The form's **Redirect URL** setting.
3. The normal html.contact success response.

Redirect URLs must be absolute `https://` URLs. For local development, `http://localhost:*` and `http://127.0.0.1:*` are also allowed. If the chosen redirect URL is missing or invalid, html.contact returns the normal success response instead of falling back to another redirect.

## Production form with one attachment

Use `multipart/form-data` when the form includes a file input. Current browser form support allows one non-empty attachment per submission.

```html
<form
  action="https://html.contact/f/hc_pub_REPLACE"
  method="POST"
  enctype="multipart/form-data"
>
  <input type="hidden" name="_replyto" value="email">

  <div
    style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;"
    aria-hidden="true"
  >
    <label for="hc-company">Company</label>
    <input
      id="hc-company"
      name="_hc_hp_company"
      type="text"
      tabindex="-1"
      autocomplete="off"
    >
  </div>

  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required>

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required>

  <label for="subject">Subject</label>
  <input id="subject" name="subject" value="Website inquiry">

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <label for="attachment">Attachment</label>
  <input id="attachment" name="attachment" type="file">

  <button type="submit">Send</button>
</form>
```

Email notifications include authenticated links to uploaded attachments instead of attaching raw files directly. You can also open the submission in the dashboard, find the **Attachments** section, then use **File** to view or download the file.

Treat every unknown upload like any other unknown attachment: download only when you expect the file, scan it with your security tools, and be cautious before opening documents, archives, or executables.

## Recipient routing

Recipients, CC/BCC, and sender behavior are configured inside html.contact or through the intended form-management API. Public fields such as `_to`, `_cc`, `_bcc`, and `_from` are stored as submitted fields only; they are not routing controls.
