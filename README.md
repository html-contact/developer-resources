# html.contact Developer Resources

Email for forms. No backend needed.

html.contact turns a normal HTML form into email notifications, submission logs, spam screening, file attachments, CSV/JSON exports, and private API access. It is built for static sites, AI-built pages, no-code sites, client projects, and small production websites where a contact form should not become a backend project.

This repository is the public developer-resource home for html.contact docs, examples, OpenAPI artifacts, and agent instructions. The html.contact application code remains the source of truth; this repo is where public, generated, and agent-readable resources can be published in a cleaner shape.

## Start Here

Use a public form key in frontend HTML:

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

Replace `hc_pub_REPLACE` with the public form key from your html.contact form.

The page that hosts the form must be added as an allowed domain in html.contact. Test from the deployed page, not only from a raw `curl` request, because browser source headers are part of the normal public form flow.

## The Two-Key Rule

| Key prefix | Where it belongs | What it does |
| --- | --- | --- |
| `hc_pub_` | Frontend HTML | Submits a public browser form to one configured form endpoint. Safe to expose in a form action. |
| `hc_live_` | Trusted server, automation, approved agent | Calls the private API with `Authorization: Bearer`. Never expose in browser code. |

Public browser forms submit `application/x-www-form-urlencoded` by default. Use `multipart/form-data` only when the form includes a file input. Public JSON request bodies are not the default public form integration.

## What This Repo Is For

The intended public layout:

```text
docs/
  Developer guides exported from the html.contact docs surface.

examples/
  Copy-paste examples for plain HTML, Astro, Next.js, Vue, Webflow, AI website builders, attachments, and production form patterns.

api/
  OpenAPI JSON/YAML artifacts and API reference material for public form submission routes and private API routes.

skill/
  Agent skill resources that help coding agents add html.contact correctly without inventing backend routes or leaking private keys.
```

The first pass is intentionally small: establish the public repo, state the boundaries, and make the safe integration path obvious. The generated docs, examples, OpenAPI files, and skill package should be synced from the application repo so the public resources do not drift away from runtime behavior.

## Public Forms

Public forms are for website visitors.

- Use `POST /f/{formKey}` with an `hc_pub_` key in the form action.
- Every submitted field you want to receive needs a `name` attribute.
- Unknown user fields are preserved.
- Recipients, CC/BCC, sender behavior, allowed domains, default subject, default intro, and redirect settings are configured in html.contact.
- Do not use hidden `_to`, `_cc`, `_bcc`, or `_from` fields for routing. They are preserved as submitted data and do not control email delivery.
- Use `_gotcha` or an `_hc_hp_*` honeypot field when you want a simple hidden bot trap.
- The default snippet does not require CAPTCHA or Turnstile.

## Private API

The private API is for trusted servers, automations, and approved agents.

Current API surface includes:

- Form listing, creation, updates, duplication, test email, and recipient verification.
- Submission lists, detail views, filters, spam folders, and CSV exports.
- Authenticated attachment downloads and previews.
- Safe usage summaries for warning users before an account reaches its submission limit.

Private API keys use `hc_live_` bearer tokens and scoped presets. API keys cannot manage billing, payment methods, invoices, account settings, linked emails, API keys, destructive dashboard actions, or private dashboard-only routes.

## Agent-Friendly Resources

html.contact is designed to be easy for coding agents and AI site builders to use safely.

Current public resources:

- Docs: <https://html.contact/docs/>
- Examples: <https://html.contact/examples/>
- Public agent guide: <https://html.contact/agents.md>
- LLM summary: <https://html.contact/llms.txt>
- Full agent context: <https://html.contact/llms-full.txt>
- OpenAPI JSON: <https://html.contact/openapi.json>
- Machine-readable pricing: <https://html.contact/pricing.md>

Agents should default to a native HTML form post. Do not create a backend route, JavaScript fetch flow, or private API integration unless the user specifically asks for one.

## Source Of Truth

The html.contact application repo owns the implementation and generated contracts:

- Public docs content lives in the app repo under `src/content/docs`.
- Generated agent and LLM files come from `src/lib/docs`.
- The OpenAPI contract comes from `src/lib/openapi/spec.ts`.
- Public form runtime behavior comes from the `/f/:formKey` and `/submit` handlers.

This public repo should publish artifacts derived from those sources. If this repo and the live product disagree, treat the live product and generated OpenAPI document as authoritative, then fix the stale public resource.

## Who This Is For

html.contact is useful when you need:

- A working contact form for a static site, landing page, portfolio, or small business site.
- A real backend for a form generated by Lovable, Bolt, v0, Replit, Cursor, Claude, Codex, or another AI builder.
- A repeatable form backend for client sites, with verified recipients, allowed domains, logs, spam screening, exports, and attachments.
- A private API for trusted server-side form and submission workflows.

It is not trying to be a drag-and-drop survey builder, CRM, payment form platform, or enterprise workflow suite. Bring your own form UI; html.contact handles the backend.

## Contributing

This repository is public, but early changes should stay tightly aligned with the app repo. Good contributions improve examples, clarify docs, or make agent instructions safer without changing the public contract by accident.

Before proposing API behavior changes, check the live OpenAPI document at <https://html.contact/openapi.json>.

## Links

- Website: <https://html.contact>
- Documentation: <https://html.contact/docs/>
- Examples: <https://html.contact/examples/>
- OpenAPI: <https://html.contact/openapi.json>
