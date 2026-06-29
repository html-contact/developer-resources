---
title: AI Website Builders
description: Prompt Lovable, Bolt, v0, Replit, Cursor, Claude, or Codex to add a working html.contact form.
---

AI builders are good at creating form UI. html.contact gives that form a real backend.

Use these prompts with a public `hc_pub_` form key. Never paste a private `hc_live_` key into an AI-built frontend.

## Simple contact form prompt

```txt
Add a contact form that posts directly to html.contact.

Endpoint:
https://html.contact/f/hc_pub_REPLACE

Requirements:
- Use a normal HTML form with method="POST".
- Include fields named name, email, and message.
- Every field that should be stored must have a name attribute. html.contact stores submitted values by field name.
- Connect labels to inputs.
- Do not create a backend route, server action, or client-side fetch.
- Do not expose any hc_live_ private API key.
- Do not use _to, _cc, _bcc, or _from for routing.
- After implementation, remind me to add the exact deployed hostname to the html.contact allowed domains.
```

Reference: [Simple HTML](/examples/simple-html-contact-form/).

## Production form prompt

```txt
Add a production-ready contact form that posts directly to html.contact.

Endpoint:
https://html.contact/f/hc_pub_REPLACE

Requirements:
- Use a normal HTML form with method="POST".
- Include name, email, subject, and message fields.
- Every field that should be stored must have a name attribute. html.contact stores submitted values by field name.
- For select, checkbox, and radio inputs, set useful value attributes because those values are what html.contact stores.
- Add a hidden input named _replyto with value email.
- Add a visually hidden empty honeypot text input whose name starts with _hc_hp_, such as _hc_hp_company. Use tabindex="-1" and autocomplete="off"; do not make it required. _gotcha is also supported for older snippets.
- Do not add recipient, CC, BCC, or sender hidden fields. Those are configured inside html.contact.
- Do not add _subject or _intro expecting them to change notifications. Use subject without the underscore for notification subject. The dashboard default intro is controlled in html.contact.
- Do not create a backend route unless I explicitly ask for server-side validation.
- Do not expose any hc_live_ private API key.
- Keep labels accessible.
- After implementation, remind me to add the exact deployed hostname to the html.contact allowed domains.
```

Reference: [Production HTML](/examples/production-html-contact-form/).

## Attachment variant prompt

```txt
Add a contact form with one file upload that posts directly to html.contact.

Endpoint:
https://html.contact/f/hc_pub_REPLACE

Requirements:
- Use a normal HTML form with method="POST".
- Add enctype="multipart/form-data".
- Include fields named name, email, message, and attachment.
- Use one file input named attachment.
- Do not add more than one file input.
- Explain that email notifications link to authenticated attachment downloads and that the user can view or download attachments from the submission in the dashboard.
- Remind the user to scan unknown downloads with their security tools before opening them.
- Do not expose any hc_live_ private API key.
- After implementation, remind me to add the exact deployed hostname to the html.contact allowed domains.
```

Reference: [Attachments](/examples/attachments/).

## Framework prompt

```txt
Add this html.contact form to the existing framework component.

Endpoint:
https://html.contact/f/hc_pub_REPLACE

Requirements:
- Render a normal browser form with method="POST".
- Do not use client-side fetch for the normal website form.
- Do not create a backend route, API route, or server action unless I explicitly ask for server-side validation.
- Include fields named name, email, and message.
- Every field that should be stored must have a name attribute. html.contact stores submitted values by field name.
- For select, checkbox, and radio inputs, set useful value attributes because those values are what html.contact stores.
- Keep the public hc_pub_ key in frontend markup.
- Never expose a private hc_live_ key.
- After implementation, remind me to add the exact deployed hostname to the html.contact allowed domains.
```

References: [Astro](/examples/astro-contact-form/), [Next.js](/examples/nextjs-contact-form/), [React](/examples/react-contact-form/), and [Vue](/examples/vue-contact-form/).

## Agent rules

- Use `hc_pub_` keys in frontend code.
- Do not use `hc_live_` keys in browser code.
- Do not create a backend route unless the user explicitly needs server-side validation first.
- Do not use `_to`, `_cc`, `_bcc`, or `_from` for routing; they are submitted fields only.
- Store fields by giving inputs a `name`; select, checkbox, and radio controls store their submitted `value`.
- Use `subject` without an underscore if the user wants the submitted form to set the notification subject.
- Do not use `_subject` or `_intro` to override notification behavior.
- Use `_replyto=email` when the form has an `email` field and the user wants replies to go to the submitter.
- Recipients, allowed domains, and spam protection are configured in html.contact.
- For server-side scripts or curl, use [Server-Side POST](/examples/http-post/).

For a deeper agent-facing reference, see [agents.md](/agents.md).
