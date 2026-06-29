---
title: Simple HTML Contact Form
description: Copy the smallest working html.contact form for a plain HTML site.
---

This is the smallest useful html.contact example. It works anywhere you can add a normal HTML form. No JavaScript, backend route, or private API key is required.

Replace `hc_pub_REPLACE` with the public form key from your form.

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

That is enough for html.contact to receive the post, store the submitted fields, send an email notification when your recipient is ready, and show the submission in your account.

## Before testing

Add the exact deployed hostname that serves the form in html.contact, then test from that deployed page. Add `example.com` and `www.example.com` separately if both serve the form.

## Keep it simple

- Do not add a private `hc_live_` key to frontend code.
- Do not create a backend route unless you need server-side validation first.
- Do not use hidden fields for recipients. Recipients are configured inside html.contact.
- Do not add `_subject` or `_intro` expecting them to change notification behavior. Use `subject` without the underscore when you want the submitted form to set the email subject.
- Every input you want to receive needs a `name` attribute.

For the recommended production version with reply-to, honeypot, and redirect options, see [Production HTML](/examples/production-html-contact-form/).
