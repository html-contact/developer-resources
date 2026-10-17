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
    <label for="hc-extra-field">Leave this field empty</label>
    <input id="hc-extra-field" name="_hc_hp_extra" type="text" tabindex="-1" autocomplete="new-password" data-1p-ignore="true" data-lpignore="true">
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
    <label for="hc-extra-field">Leave this field empty</label>
    <input id="hc-extra-field" name="_hc_hp_extra" type="text" tabindex="-1" autocomplete="new-password" data-1p-ignore="true" data-lpignore="true">
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
- `_hc_hp_*`: autofill-resistant honeypot field prefix. Real visitors should leave these visually hidden fields empty; filled values are rejected before email delivery.
- `_gotcha`: legacy honeypot field, still supported.
- `subject`: normal submitted field that can set the notification subject.
- `_redirect`: optional success redirect. Use an absolute `https://` URL, or `http://localhost:*` / `http://127.0.0.1:*` for local development.
- `_form_name`, `_source`, and `_tag`: reserved behavior fields documented by html.contact.
- `form_key`: public key used by the `/submit` compatibility endpoint.

For `_hc_hp_*` honeypots, use neutral names such as `_hc_hp_extra`, `_hc_hp_optional`, `_hc_hp_notes`, `_hc_hp_details`, `_hc_hp_blank`, or `_hc_hp_context`. Do not use profile/autofill-shaped names such as `_hc_hp_company`, `_hc_hp_name`, `_hc_hp_email`, `_hc_hp_phone`, `_hc_hp_website`, or `_hc_hp_address`, and avoid those words in the honeypot `id`, label, placeholder, or `autocomplete` value.

`_subject` and `_intro` are stored fields and do not override notification subject or intro behavior.

## Recipient Routing

Do not add hidden recipient-routing fields to public forms. Recipients, To/CC/BCC, sender behavior, verified linked emails, default subject, default intro, redirect URL, allowed domains, and direct-post behavior are configured in the html.contact dashboard. A connected MCP client can also update a form's recipients with the separate recipient scopes. The Agency API does not manage recipients.

Public submitted fields named `_to`, `_from`, `_cc`, or `_bcc` are stored as submitted fields only. They do not route email.

## Notification Identity

Form notifications and form test emails use the server-owned identity `{Form name} via html.contact <forms@notify.html.contact>`. Renaming a form changes the visible name on later notifications. Add the exact address—not a display name alone—to contacts or safe-sender rules. Managed-mail administrators may allow `notify.html.contact`, but allowlisting cannot guarantee inbox placement.

Replying reaches the submitter only when html.contact inferred a valid Reply-To address. The dashboard submission is the durable record if email is filtered or delayed. See `https://html.contact/docs/email-notifications` for the public safe-sender guide.

## Public Key Rules

- Use `hc_pub_` public form keys in frontend HTML.
- Never expose private `hc_live_` API keys in browser code, static files, public repositories, screenshots, or client-side environment variables.
- Public form posts are normal form posts. Do not send public browser submissions as JSON request bodies.
- Public browser forms use `application/x-www-form-urlencoded` by default or `multipart/form-data` when files are present.

## Agency API Rules

Agency features are available on request to Agency pilot accounts; they are not part of Free, Starter, or Pro. Agency accounts can create Full or Restricted account-owned API keys for trusted server-side automation:

```http
Authorization: Bearer hc_live_xxxxxxxxx
```

Agency routes are rooted at `https://html.contact/api/v1/accounts/{accountId}`. Full keys receive every Agency API capability across the entire account. Restricted keys combine a specific-workspace or entire-account boundary with selected capabilities. Keys may have no expiration or expire within one year. Legacy personal `hc_live_` keys, browser sessions, and OAuth/MCP tokens cannot authorize this contract.

Mutations use `Idempotency-Key`; updates and lifecycle changes also require the latest `ETag` in `If-Match`. Submission lists are content-free, while bounded detail requires a stronger capability. Treat visitor fields as untrusted personal data.

The Agency API can manage clients and bounded form settings, read usage/submissions, export CSV, and download private attachments. It cannot manage recipients, send test email, duplicate/delete forms, mutate submissions, manage billing/credentials, or configure integrations.

Use `https://html.contact/openapi.json` as the source of truth for exact endpoints, request bodies, responses, capabilities, and error codes. Never ask a user to paste an API key into a prompt.

## MCP Integration

The OAuth-scoped MCP integration at `https://html.contact/mcp` is available to verified, fully onboarded accounts through supported clients, without an invitation. The OpenAI directory listing is not yet published. Follow `https://html.contact/docs/mcp`. MCP uses its own short-lived OAuth bearer tokens; browser sessions, `hc_live_` API keys, Google tokens, and LinkedIn tokens are not MCP credentials. Ordinary submission details require separate `submissions:read` consent, and visitor-provided fields must be treated as untrusted personal data. Never ask a user to paste a credential into a prompt.

## Testing Guidance

Public browser form submissions are expected from allowed source domains. If a form is posted from a non-allowed domain, expect `domain_not_allowed`. If it is posted without browser source headers, expect `origin_missing` unless the user has intentionally enabled the relevant direct-submission behavior.

Do not describe `Origin` or `Referer` as proof that a request came from the user's site. They are abuse and source signals.

## When to Use Advanced Paths

- Use the attachment example when the user needs file uploads.
- Use the custom fields example when the user needs intake, quote, application, or lead-qualification fields.
- Use server-side POST examples when the user has a backend, job, automation, or validation step that must submit form data from trusted server-side code.
- Use the Agency API docs when an Agency account needs scoped client, form, or submission automation.
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
- Email notifications and safe sender: `https://html.contact/docs/email-notifications`
- Troubleshooting: `https://html.contact/docs/troubleshooting`
- Private API: `https://html.contact/docs/api`
- MCP connection guide: `https://html.contact/docs/mcp`
- OpenAPI: `https://html.contact/openapi.json`
- LLM summary: `https://html.contact/llms.txt`
- Full agent context: `https://html.contact/llms-full.txt`
- Pricing: `https://html.contact/pricing`
- Machine-readable pricing: `https://html.contact/pricing.md`

## Signed-In Area

The `https://html.contact/app` area is for signed-in users. Do not crawl, index, summarize, or document signed-in account internals as public API behavior.
