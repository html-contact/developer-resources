---
title: AI Website Builders
description: Prompt Lovable, Bolt, v0, Replit, Cursor, Claude, or Codex to add a working html.contact form.
---

AI builders are good at creating form UI. html.contact gives that form a real backend.

Use these prompts with a public `hc_pub_` form key. Never paste a private `hc_live_` key into an AI-built frontend.

## Default prompt

```txt
Add a working contact form to this page with html.contact.

Use this form endpoint:
https://html.contact/f/hc_pub_REPLACE

Build a normal HTML form with method="POST", accessible labels, and fields named:
name, email, message

Use a real submit button:
<button type="submit">Send</button>

Rules:
- Do not use fetch, an API route, server action, or backend function.
- Do not expose any hc_live_ private API key.
- Do not use _to, _cc, _bcc, or _from; recipients are configured in html.contact.
- Every field that should be stored must have a name attribute.
- After implementing, remind me to add the deployed hostname to allowed domains in html.contact.
```

Reference: [Simple HTML](simple-html-contact-form.md).

## Fixed action fallback

```txt
Add a working html.contact form using the generic submit endpoint.

Set the form action to:
https://html.contact/submit

Set method="POST".

Add this hidden field:
<input type="hidden" name="form_key" value="hc_pub_REPLACE">

Include accessible fields named:
name, email, message

Rules:
- Do not expose any hc_live_ private API key.
- Do not use _to, _cc, _bcc, or _from; recipients are configured in html.contact.
- Every stored field needs a name attribute.
- After implementing, remind me to add the deployed hostname to allowed domains in html.contact.
```

Use this fallback when a website builder only lets you set one fixed submit URL or makes it easier to add hidden fields than to change the form action path.

## Production add-on

```txt
Make the html.contact form production-ready.

Add:
- A subject field named subject.
- A hidden reply-to field: <input type="hidden" name="_replyto" value="email">
- A visually hidden empty honeypot field named _hc_hp_extra.

Honeypot rules:
- Use type="text".
- Use tabindex="-1".
- Use autocomplete="new-password" plus data-1p-ignore="true" and data-lpignore="true".
- Do not make it required.
- Hide it visually, but keep it in the form markup.
- Use neutral honeypot names such as _hc_hp_extra, _hc_hp_optional, _hc_hp_notes, _hc_hp_details, _hc_hp_blank, or _hc_hp_context.
- Do not use profile/autofill words like company, name, email, phone, website, or address in the honeypot name, id, label, placeholder, or autocomplete value.
- Avoid honeypot names such as _hc_hp_company, _hc_hp_name, _hc_hp_email, _hc_hp_phone, _hc_hp_website, or _hc_hp_address because browsers and password managers may autofill them.

For select, checkbox, and radio inputs, use meaningful value attributes.
```

Reference: [Production HTML](production-html-contact-form.md).

## Attachment add-on

```txt
Add one file upload to the html.contact form.

Requirements:
- Add enctype="multipart/form-data" to the form.
- Add one file input named attachment.
- Keep the existing name, email, and message fields.
- Do not add more than one file input unless I ask.
- Explain that attachment links require authenticated dashboard access.
- Remind me to scan unknown downloads with my security tools before opening them.
```

References: [Attachments](attachments.md) and [File Upload Form](https://html.contact/forms/file-upload-form/).

## Framework prompt

```txt
Add this html.contact form to the existing framework component.

Use:
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">

Render an actual browser form with fields named:
name, email, message

Rules:
- Do not convert this to fetch.
- Do not create an API route, server action, or backend function.
- Keep the public hc_pub_ key in frontend markup.
- Never expose an hc_live_ private API key.
- Use accessible labels and a submit button.
- After implementing, remind me to add the deployed hostname to allowed domains in html.contact.
```

References: [Astro](astro-contact-form.md), [Next.js](nextjs-contact-form.md), [React](react-contact-form.md), and [Vue](vue-contact-form.md).

## Coding agent prompt

```txt
Update this app to add a working html.contact form.

Find the existing contact form or contact page. If one does not exist, create a simple accessible contact form.

Use:
action="https://html.contact/f/hc_pub_REPLACE"
method="POST"

Fields:
- name
- email
- message

Constraints:
- Use normal browser form submission.
- Do not add fetch, an API route, server action, or email-sending dependency.
- Do not expose any hc_live_ key.
- Do not use _to, _cc, _bcc, or _from.
- Preserve the existing design system and styling patterns.
- Run the project's relevant type/build checks after the change.
```

Use this with coding agents such as Cursor, Claude, Codex, or Replit Agent when they can inspect and edit an existing codebase.

## Agent rules

- Use `hc_pub_` keys in frontend code.
- Do not use `hc_live_` keys in browser code.
- Prefer `https://html.contact/f/hc_pub_REPLACE` for normal forms.
- Use `https://html.contact/submit` plus `<input type="hidden" name="form_key" value="hc_pub_REPLACE">` only when the builder needs one fixed submit URL or handles hidden fields more reliably than path-based actions.
- Do not create a backend route unless the user explicitly needs server-side validation first.
- Do not use `_to`, `_cc`, `_bcc`, or `_from` for routing; they are submitted fields only.
- Store fields by giving inputs a `name`; select, checkbox, and radio controls store their submitted `value`.
- Use `subject` without an underscore if the user wants the submitted form to set the notification subject.
- Do not use `_subject` or `_intro` to override notification behavior.
- Use `_replyto=email` when the form has an `email` field and the user wants replies to go to the submitter.
- Use autofill-resistant `_hc_hp_*` honeypots such as `_hc_hp_extra`, not profile-shaped names such as `_hc_hp_company`.
- Recipients, allowed domains, and spam protection are configured in html.contact.
- For server-side scripts or curl, use [Server-Side POST](http-post.md).

For a deeper agent-facing reference, see [agents.md](https://html.contact/agents.md).
