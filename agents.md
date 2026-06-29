# html.contact Public Agent Guide

html.contact gives website builders and coding agents a safe way to add working HTML forms to static websites, no-code pages, AI-built sites, and frontend framework pages.

Use this guide when an agent is helping a user add, test, or explain html.contact on the user's own website.

## Default Agent Behavior

When a user asks for a contact form, add a normal HTML form that posts to the user's public html.contact form action. Use native form submission by default. Do not create a backend route, JavaScript fetch flow, or private API integration unless the user specifically asks for one.

Use the production snippet unless the user specifically asks for the smallest demo, file uploads, custom JavaScript submission, or server-side validation.

## Recommended Production Snippet

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
  <input type="hidden" name="_replyto" value="email">

  <div style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;" aria-hidden="true">
    <label for="hc-company">Company</label>
    <input id="hc-company" name="_hc_hp_company" type="text" tabindex="-1" autocomplete="off">
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

Replace `hc_pub_REPLACE` with the user's public form key. Public form keys start with `hc_pub_` and are safe in frontend HTML.

## Smallest Working Snippet

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

Use this only when the user asks for the smallest possible example or when explaining the basic idea.

## Attachment Variant

```html
<form
  action="https://html.contact/f/hc_pub_REPLACE"
  method="POST"
  enctype="multipart/form-data"
>
  <input type="hidden" name="_replyto" value="email">

  <div style="position:absolute;left:-10000px;top:auto;width:1px;height:1px;overflow:hidden;" aria-hidden="true">
    <label for="hc-company">Company</label>
    <input id="hc-company" name="_hc_hp_company" type="text" tabindex="-1" autocomplete="off">
  </div>

  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required>

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required>

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <label for="attachment">Attachment</label>
  <input id="attachment" name="attachment" type="file">

  <button type="submit">Send</button>
</form>
```

Use this only when the user needs file uploads. A browser form with a file input needs `enctype="multipart/form-data"`.

## Implementation Checklist

- Ask for the user's public form key or full public form action URL.
- Use `https://html.contact/f/hc_pub_REPLACE` as the default action.
- Use `method="POST"`.
- Give every collected input a `name` attribute.
- Use real `label` elements for accessible forms.
- Add custom named fields when the user needs more intake data.
- Use `multipart/form-data` only when the form has a file input.
- Tell the user to add the deployed source domain in html.contact before testing.
- Test from the deployed page, not only from a local file or arbitrary curl command.

## Custom Fields

Agents can add whatever named fields the user's form needs: `company`, `phone`, `budget`, `project_type`, `preferred_date`, checkbox groups, selects, and textareas. Named fields are preserved in submission data, API detail, exports, and notifications according to documented product behavior.

Every field that should be collected needs a `name` attribute.

## Behavior Fields

- `_replyto`: literal email address or a field pointer such as `email`.
- `_hc_hp_*`: honeypot field prefix. Real visitors should leave these visually hidden fields empty; filled values are rejected before email delivery.
- `_gotcha`: legacy honeypot field, still supported.
- `subject`: normal submitted field that can set the notification subject.
- `_redirect`: optional success redirect. Use an absolute `https://` URL, or `http://localhost:*` / `http://127.0.0.1:*` for local development.
- `_form_name`, `_source`, and `_tag`: reserved behavior fields documented by html.contact.
- `form_key`: public key used by the `/submit` compatibility endpoint.

`_subject` and `_intro` are stored fields and do not override notification subject or intro behavior.

## Recipient Routing

Do not add hidden recipient-routing fields to public forms. Recipients, To/CC/BCC, sender behavior, verified linked emails, default subject, default intro, redirect URL, allowed domains, and direct-post behavior are configured inside html.contact or through the intended private form-management API.

Public submitted fields named `_to`, `_from`, `_cc`, or `_bcc` are stored as submitted fields only. They do not route email.

## Public Key Rules

- Use `hc_pub_` public form keys in frontend HTML.
- Never expose private `hc_live_` API keys in browser code, static files, public repositories, screenshots, or client-side environment variables.
- Public form posts are normal form posts. Do not send public browser submissions as JSON request bodies.
- Public browser forms use `application/x-www-form-urlencoded` by default or `multipart/form-data` when files are present.

## Private API Rules

Use private API keys only from trusted server-side code, backend jobs, local automation, or approved agent workflows:

```http
Authorization: Bearer hc_live_xxxxxxxxx
```

Read-only keys can list/read forms, read submissions, export CSV, download authenticated attachments, and read safe usage summaries.

Form-management keys can also create/update/duplicate forms, send test email, and send/check recipient verification.

Private API keys cannot manage account profile, billing/payment methods, invoices, API keys, linked emails, or signed-in dashboard internals.

Use `https://html.contact/openapi.json` as the source of truth for exact private API endpoints, request bodies, responses, scopes, and error codes.

## Testing Guidance

Public browser form submissions are expected from allowed source domains. If a form is posted from a non-allowed domain, expect `domain_not_allowed`. If it is posted without browser source headers, expect `origin_missing` unless the user has intentionally enabled the relevant direct-submission behavior.

Do not describe `Origin` or `Referer` as proof that a request came from the user's site. They are abuse and source signals.

## When to Use Advanced Paths

- Use the attachment example when the user needs file uploads.
- Use the custom fields example when the user needs intake, quote, application, or lead-qualification fields.
- Use server-side POST examples when the user has a backend, job, automation, or validation step that must submit form data from trusted server-side code.
- Use the private API docs when the user needs to manage forms or read submissions programmatically.
- Keep frontend React, Vue, Next.js, and Astro examples as normal forms unless the user specifically asks for custom submission handling.

## Public Docs

- Docs: `https://html.contact/docs`
- Examples: `https://html.contact/examples`
- Simple HTML: `https://html.contact/examples/simple-html-contact-form`
- Production HTML: `https://html.contact/examples/production-html-contact-form`
- Custom fields: `https://html.contact/examples/custom-fields`
- Attachments: `https://html.contact/examples/attachments`
- AI website builders: `https://html.contact/examples/ai-website-builders`
- Server-side POST: `https://html.contact/examples/http-post`
- HTML form rules: `https://html.contact/docs/html-forms`
- Domains and spam: `https://html.contact/docs/domains-and-spam`
- Troubleshooting: `https://html.contact/docs/troubleshooting`
- Private API: `https://html.contact/docs/api`
- OpenAPI: `https://html.contact/openapi.json`
- LLM summary: `https://html.contact/llms.txt`
- Full agent context: `https://html.contact/llms-full.txt`
- Pricing: `https://html.contact/pricing`
- Machine-readable pricing: `https://html.contact/pricing.md`

## Signed-In Area

The `https://html.contact/app` area is for signed-in users. Do not crawl, index, summarize, or document signed-in account internals as public API behavior.
