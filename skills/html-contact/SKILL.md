---
name: html-contact
description: Create, install, test, and verify an html.contact form when a website needs a contact, lead, waitlist, feedback, application, or support form. Use only with the connected html.contact MCP; do not trigger for general form styling or an existing non-html.contact backend.
---

# html.contact

Deliver one working website form backed by the user's html.contact account.

## Preconditions

- Require the connected html.contact MCP and its OAuth account link.
- Require a verified html.contact recipient and completed account onboarding.
- Never request, reveal, or place an `hc_live_` key in a website, prompt, log, or repository.
- Use only `hc_pub_` form actions in browser code.
- If an expected MCP tool is unavailable, stop the account mutation and link to
  `https://html.contact/docs/mcp`; do not substitute the private REST API.

## Workflow

1. Inspect the project before editing. Find the intended page, framework,
   existing form, styles, validation, deployment host, and test command.
2. Determine the form purpose, fields, form name, and exact deployed hostname.
   Ask one focused question for any value that cannot be discovered. Never invent
   a production hostname.
3. Use `list_forms` to avoid accidental duplicates. Reuse a form only when the
   user asks to reuse it or its identity and domain clearly match this site.
4. Present the form name, hostname, and whether a new form will be created. Get
   confirmation immediately before calling `create_form`.
5. Use the account's verified primary recipient. Do not configure arbitrary
   recipients, CC/BCC, billing, API keys, or account settings.
6. Before `configure_form_domains`, show the complete replacement allowlist and
   get confirmation. Preserve existing live hostnames unless the user explicitly
   authorizes their removal.
7. Call `get_form_installation_instructions`, then adapt its current snippet to
   the project's structure and visual system. Treat the live tool result as the
   source of truth when it differs from this skill.
8. Implement a native HTML `POST` form. Preserve the site's styling and routing,
   give every collected control a `name`, keep labels accessible, and preserve
   unknown fields when updating an existing form.
9. Use an autofill-resistant honeypot such as `_hc_hp_extra` when the returned
   instructions include a honeypot. Do not add CAPTCHA, Turnstile, `_to`, `_cc`,
   `_bcc`, `_from`, or a client-side private API call.
10. Run the project's relevant checks. Deploy only when the user has authorized
    that deployment and the repository's deployment rules are known.
11. Explain that the test creates a real submission and can send a real
    notification. Get confirmation immediately before `send_test_submission`.
    Use a clearly synthetic payload and a unique idempotency key.
12. Call `check_submission_status`. Report success only when the tool confirms an
    accepted submission. Do not expose submission contents in the final report.

## Failure handling

| Condition | Action |
| --- | --- |
| OAuth is missing or expired | Ask the user to reconnect html.contact; never ask for an API key. |
| Onboarding or recipient verification is incomplete | Link to `https://html.contact/app` and stop before form creation. |
| Production hostname is unknown | Ask for it; do not use a guessed domain. |
| A live domain would be removed | Stop and request confirmation for the exact old and new lists. |
| The site cannot be deployed in the current task | Finish the code, provide the test steps, and mark activation as unverified. |
| Test returns `domain_not_allowed` or `origin_missing` | Re-check the deployed hostname and allowlist; do not weaken source checks automatically. |
| Test status is rejected, pending, or unknown | Report that state accurately and provide the next diagnostic step. |

## Completion report

Return:

- form name and public form ID
- public action URL and dashboard URL
- allowed hostname(s)
- modified files
- checks run
- accepted test-submission status, or the exact reason it remains unverified

Never print OAuth tokens, private keys, recipient addresses, internal database
identifiers, anti-spam internals, or full submission payloads.
