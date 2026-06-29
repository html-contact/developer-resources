---
title: Add a Contact Form
description: Create your first html.contact endpoint and connect it to a plain HTML form.
---

This is the shortest path from a website page to a working html.contact inbox.

## 1. Create a form

Create a form in html.contact. The form workspace shows a public form action URL like:

```txt
https://html.contact/f/hc_pub_REPLACE
```

Replace `hc_pub_REPLACE` with the public form key from your form. Public form keys start with `hc_pub_` and are safe to put in frontend HTML.

## 2. Choose the recipient

Choose the inbox that should receive submissions. If html.contact asks you to verify that recipient, finish verification before expecting live email delivery.

Recipients, CC/BCC, and sender behavior are configured inside html.contact. Do not add hidden public form fields to route email.

## 3. Add your website hostname

Add the exact hostname that serves the page with the form.

Examples:

- `example.com`
- `www.example.com`
- `contact.example.com`

If both `example.com` and `www.example.com` serve the form, add both.

## 4. Paste the form

Use your public endpoint as the form action and submit with `method="POST"`. Every input you want to receive needs a `name` attribute.

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

## 5. Test the flow

Publish the page and submit the form from the hostname you added. You should receive an email notification and see the submission in html.contact.

Testing from a different site can return `domain_not_allowed`. Testing with a raw server or curl request can return `origin_missing` unless direct submissions are enabled for that form.

## Next steps

- Add a production-ready honeypot and reply-to field with [Production HTML](/examples/production-html-contact-form/).
- Add arbitrary named fields with [Custom Fields](/examples/custom-fields/).
- Add a file input with [Attachments](/examples/attachments/).
- Check field behavior in [Form Fields](/docs/form-fields/).
- Diagnose setup issues with [Troubleshooting](/docs/troubleshooting/).
