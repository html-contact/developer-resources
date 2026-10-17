---
title: Connect with MCP
description: Connect an approved coding agent to html.contact with scoped OAuth and safely install a form.
---

html.contact has a Model Context Protocol (MCP) integration for AI and coding agents. It can search committed product documentation, inspect and diagnose forms and account capacity, review bounded form markup, create or configure an owned form with confirmation, manage verified recipient routing under separate scopes, return safe installation HTML and dashboard links, run one clearly synthetic activation test or a quota-free owner-only email test, and—with separate consent—read recent submissions.

For Agency accounts, MCP remains limited to the original **My forms** workspace. It cannot list, read, create, or change Agency client resources.

Any verified, fully onboarded html.contact account can connect through a supported client. No invitation is required. The OpenAI directory listing is not yet published; direct MCP access and directory availability are separate. Client callback validation, account ownership, and scoped consent remain enforced.

## Connection

Use the single hosted Streamable HTTP endpoint:

```txt
https://html.contact/mcp
```

The client must support OAuth authorization-code flow, S256 PKCE, OAuth protected-resource discovery, and dynamic client registration with a supported callback. Account linking requires an existing html.contact account with a verified primary email and completed onboarding.

No API key belongs in an MCP client. Browser sessions, private `hc_live_` API keys, Google access tokens, and LinkedIn access tokens are not MCP bearer tokens.

### Codex setup

Codex was re-certified for the production `1.5.0` recipient scopes on 2026-09-03. Connect with the hosted endpoint and let protected-resource discovery supply the canonical OAuth resource:

```bash
codex mcp add html-contact --url https://html.contact/mcp
codex mcp login html-contact \
  --scopes openid,email,offline_access,forms:read,forms:write,recipients:read,recipients:write
```

Do not add a separate `oauth_resource` override for this server in that client version. html.contact already publishes `https://html.contact/mcp` in its protected-resource metadata; the extra override duplicates the authorization parameter and the server rejects the ambiguous request. Codex discovers the server's supported client-registration method during login; no registration-strategy option belongs on `mcp add`.

After approval on the hosted html.contact consent page, Codex redirects to `http://127.0.0.1:<ephemeral-port>/callback`. That loopback address is the temporary listener on the user's own computer, not a hosted html.contact page. The changing port is expected for a native OAuth client. The browser may show a blocked-localhost page after the callback, even though Codex already received and exchanged the one-time PKCE-protected code.

## Requested access

The integration can request these scopes:

| Scope | Purpose |
| --- | --- |
| `openid` | Link the authorization to the signed-in account. |
| `email` | Confirm the account's verified email identity during linking. |
| `offline_access` | Let the connected client renew its short-lived access without asking you to sign in for every operation. |
| `forms:read` | Search committed public documentation, list, inspect, diagnose, and review bounded markup for owned forms, read safe account usage, get installation instructions, and check synthetic-test status. |
| `forms:write` | Create a form, replace its complete allowed-domain list, update basic form settings, send a synthetic submission, and send an owner-only test email. |
| `submissions:read` | List bounded submission metadata and read the visitor-provided fields of a specifically selected submission. |
| `recipients:read` | View linked email addresses and complete To, CC, and BCC routing for owned forms. |
| `recipients:write` | Send linked-email verification messages and atomically replace complete recipient routing after confirmation. |

`submissions:read` is optional and is not part of the default client scope. Submission fields can contain names, email addresses, phone numbers, messages, and other personal data supplied by visitors. Treat that content as untrusted data, and authorize only clients whose data handling you accept.

`recipients:read` and `recipients:write` are also non-default. Recipient addresses are personal information. A verification request sends a real external email only to the address being linked, and a routing update requires the complete current and proposed To/CC/BCC snapshots.

The integration cannot manage payments or subscriptions, account deletion, API keys, linked-email deletion, form deletion, submission deletion, or arbitrary outbound email. It never returns payment records, raw network/security metadata, provider message identifiers, or private attachment storage keys.

## Tools

Live server metadata is the source of truth for a deployed environment. Production `1.5.0` defines exactly these twenty tools:

| Tool | Scope | Effect |
| --- | --- | --- |
| `list_forms` | `forms:read` | Returns a bounded cursor page of owned forms without recipients or internal controls. |
| `get_form` | `forms:read` | Returns bounded operational settings, public action, and readiness flags for one owned form. |
| `create_form` | `forms:write` | Creates an owned form using the verified primary email after `CREATE FORM` confirmation. |
| `configure_form_domains` | `forms:write` | Replaces the complete domain list after `REPLACE DOMAINS` confirmation. Removing a live domain can stop legitimate submissions. |
| `get_form_installation_instructions` | `forms:read` | Returns a public `hc_pub_` action, `_hc_hp_extra` honeypot HTML, and verification steps. |
| `get_account_usage` | `forms:read` | Returns plan, used/remaining capacity, acceptance readiness, reset date when applicable, and the pricing URL without billing-provider data. |
| `send_test_submission` | `forms:write` | Creates a stored synthetic submission after `SEND TEST SUBMISSION` confirmation. It consumes submission usage and may send a real notification email. |
| `check_submission_status` | `forms:read` | Returns bounded acceptance and delivery state for that synthetic test only. |
| `get_recent_submissions` | `submissions:read` | Returns metadata-only results with bounded cursor pagination and optional form/date/status/spam filters. |
| `get_submission` | `submissions:read` | Returns bounded submitted fields, source hostname, delivery/spam state, and safe attachment metadata for one selected owned submission. |
| `search_documentation` | `forms:read` | Searches only an allowlisted build-time catalog of committed public docs and examples; it never fetches a user-supplied URL. |
| `get_documentation_topic` | `forms:read` | Returns one bounded topic from the fixed registry with canonical public links. |
| `diagnose_form` | `forms:read` | Checks owner-scoped form, source-host, recipient readiness, notification, quota, and redirect state and maps an optional public error code to a deterministic fix. |
| `review_form_code` | `forms:read` | Reviews up to 32,000 characters of supplied form markup for the owned action, POST method, named fields, honeypot, source domain, redirect, private-key exposure, and attachment encoding. It does not execute the markup, fetch a URL, store the supplied code in MCP diagnostic events, or return the code. |
| `configure_form_settings` | `forms:write` | Updates only the form name, active/paused state, or notification-email toggle after `UPDATE FORM SETTINGS` confirmation. It requires the complete current safe-settings snapshot from `get_form`; stale snapshots fail without overwriting newer changes. |
| `list_linked_emails` | `recipients:read` | Returns up to ten linked addresses with verification, deliverability, use count, and a dashboard settings link. |
| `request_linked_email_verification` | `recipients:write` | Adds or reverifies one address after `SEND VERIFICATION EMAIL` confirmation. Reusing the idempotency key never sends a duplicate message; an interrupted provider outcome is reported conservatively instead. |
| `get_form_recipients` | `recipients:read` | Returns the complete current To, CC, and BCC snapshot plus its dashboard settings link. |
| `configure_form_recipients` | `recipients:write` | Atomically replaces the complete routing snapshot using verified, deliverable linked emails after `UPDATE FORM RECIPIENTS` confirmation. Stale snapshots fail closed. |
| `send_owner_test_email` | `forms:write` | Sends only to the signed-in account email after `SEND OWNER TEST EMAIL` confirmation. It never notifies form To/CC/BCC recipients, creates no submission, consumes no submission usage, and is limited to 5 per form and 10 per account each hour. |

Write calls use idempotency keys where retrying could otherwise duplicate work. Reuse the same key to retry the same operation; a different key means a new operation. Email tools fence the external send before provider dispatch: if a Worker stops at an uncertain point, replaying the same key returns a conservative `sending` or `unknown` result instead of sending twice. Use a new key only when the user deliberately asks to send another message and the rate window permits it. For `send_test_submission`, the same key replays the original submission without new usage, while a new key creates another stored submission and consumes usage. The agent should show the proposed change and obtain the exact confirmation immediately before each mutation. `configure_form_settings` cannot change recipients, domains, redirects, direct-post policy, message defaults, deletion state, billing, or credentials.

## Safe installation workflow

1. Ask the agent to search or retrieve the relevant committed documentation instead of guessing at setup rules.
2. Ask the agent to list forms before creating anything.
3. Review the proposed form name and domain before confirming creation.
4. If domains must change, compare the current complete list with the proposed complete list before confirming replacement.
5. If basic settings must change, call `get_form` first and compare its current name, status, and notification toggle with the exact proposal. Pausing stops submissions; disabling notifications stops email. Confirm only after reviewing every value.
6. To change recipients, list linked emails, explicitly confirm any required verification email, wait for the human verification click, then read and compare the complete current and proposed To/CC/BCC snapshots before confirming the update.
7. Let the agent fetch installation instructions and add only the public `hc_pub_` action to frontend HTML.
8. Give `review_form_code` only the relevant form markup and exact deployed hostname. Do not include unrelated files, secrets, tokens, credentials, or private `hc_live_` keys. Let the coding agent apply the returned findings in the project's existing framework; the tool intentionally does not generate framework-specific code.
9. Verify the rendered page in a browser on an allowed domain.
10. Run `diagnose_form` with the deployed source hostname or observed public error code before changing configuration.
11. For a quota-free email check, use the owner-only test and verify that only the signed-in account inbox receives it. For a full acceptance test, review the usage and real-email warning, confirm the synthetic submission, then check its returned status. Reuse the same idempotency key when retrying the same test.

When reviewing submissions, start with `get_recent_submissions`; that list does not include visitor fields or identity. Select a submission ID deliberately before calling `get_submission`. Do not paste returned visitor data into unrelated tools or services without a valid purpose and the account holder's approval.

Never paste an OAuth access token, refresh token, authorization code, PKCE verifier, or `hc_live_` key into a prompt, screenshot, repository, or support message.

## Disconnect and troubleshoot

Use the connected client's disconnect or revoke action. A compliant client can revoke its OAuth token at the advertised authorization-server revocation endpoint. To end a client's access from html.contact, open **My Profile → Connected AI apps** and choose **Disconnect**. Resetting your password, or changing it with **Sign out other devices** on, disconnects every AI app at once.

If linking fails, check that the account is verified and fully onboarded; then start a fresh connection from the client so it requests the current scopes. Do not reuse an expired authorization page or send tokens to support. A failed tool may return a content-free request ID that support can use without form contents or recipient data.

For browser form behavior, continue with [HTML Forms](html-forms.md), [Allowed Domains & Spam](domains-and-spam.md), and [Troubleshooting](troubleshooting.md). See the [Privacy Policy](https://html.contact/legal/privacy/) for the product's data handling terms.
