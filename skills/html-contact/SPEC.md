# html.contact Skill Specification

## Intent

Guide a coding agent through the complete activation path: create one owned
html.contact form, install it into the user's website, and confirm one accepted
test submission through the hosted html.contact MCP.

## Scope

In scope:

- contact, lead, waitlist, feedback, application, and support forms
- owner-authorized form creation and allowed-domain configuration
- native website-form integration that uses an `hc_pub_` action
- a synthetic test submission and accepted-status check
- safe failure reporting when deployment or MCP access is unavailable

Out of scope:

- billing, subscriptions, account administration, API-key management, and deletion
- arbitrary recipient, CC, or BCC changes
- reading ordinary customer submissions or exposing their contents
- replacing an existing non-html.contact backend without explicit user direction
- embedding MCP server implementation or credentials in the skill package

## Users And Trigger Context

- Primary users: website owners and coding agents adding a working form backend.
- Common requests: add a contact form, wire up a waitlist, make this feedback form
  work, create a lead form, or install html.contact on this site.
- Should not trigger for: CSS-only form changes, validation-only changes, survey or
  payment-form products, or projects already using another backend unless the user
  explicitly asks to migrate to html.contact.

## Runtime Contract

- Required first actions: inspect the project, identify the exact hostname, verify
  MCP/OAuth availability, and check existing forms.
- Required outputs: public form identity and URLs, modified files, validation run,
  and accepted test status or a precise unverified reason.
- Non-negotiable constraints: owner scoping, explicit confirmation before MCP
  writes and real test notification, idempotent writes, no `hc_live_` browser
  secret, no arbitrary recipient configuration, and no false success claim.
- Expected bundled files loaded at runtime: only `SKILL.md`.

## Source And Evidence Model

Authoritative sources:

- the live html.contact MCP tool schemas and results
- `https://html.contact/docs/mcp`
- `https://html.contact/openapi.json` for public form behavior only
- the Agent Skills specification

Useful improvement sources:

- positive examples: complete form creation plus accepted test
- negative examples: duplicate forms, wrong domains, exposed private keys, or
  unverified completion claims
- validation results: structural validator plus forward tests in supported clients

Do not store secrets, OAuth tokens, API keys, recipient addresses, customer
submissions, reviewer credentials, or private account identifiers as evidence.

## Reference Architecture

- `SKILL.md` contains the complete portable runtime workflow.
- `SPEC.md` contains scope, maintenance, and validation policy.
- `SOURCES.md` contains provenance, decisions, coverage, and open gaps.
- No runtime references, scripts, or assets are needed for the initial skill.

## Validation

- Run the Agent Skills structural validator.
- Test should-trigger and should-not-trigger request sets.
- Forward-test create, existing-form, missing-auth, missing-domain,
  domain-replacement, unavailable-deploy, rejected-test, and accepted-test paths.
- Validate separately in each marketplace wrapper because MCP configuration and
  OAuth behavior are client-specific.

Acceptance gates:

- the skill never requests an `hc_live_` key
- it cannot claim completion without an accepted test status
- it does not remove a live domain without exact confirmation
- it emits only public form identity and bounded completion metadata

## Known Limitations

- The skill cannot run until the production MCP and named tools exist.
- A synthetic MCP test proves html.contact acceptance, not the deployed page's
  visual behavior; browser testing remains necessary when the page is live.
- OAuth and remote-MCP behavior must be certified per client before listing.

## Maintenance Notes

- Update `SKILL.md` when the live tool contract or safety boundary changes.
- Update `SOURCES.md` when marketplace documentation, test evidence, or deferred
  decisions change.
- Keep platform manifests outside this skill directory so the runtime guidance
  stays vendor-neutral.
