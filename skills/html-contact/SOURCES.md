# html.contact Skill Sources

Checked: 2026-09-02

## Source inventory

| Source | Trust | Contribution | Usage |
| --- | --- | --- | --- |
| `https://html.contact/agents.md` | Product authority | Public key, form, domain, recipient, and testing rules | Adapt behavior; do not copy stale snippets blindly |
| `https://html.contact/docs/getting-started` | Product authority | End-to-end browser form workflow | Runtime workflow |
| `https://html.contact/docs/domains-and-spam` | Product authority | Allowed-domain and honeypot constraints | Safety and failure handling |
| `https://html.contact/docs/troubleshooting` | Product authority | Rejection states and repair paths | Failure handling |
| `https://agentskills.io/specification` | Standard authority | Portable `SKILL.md` format | Packaging and validation |
| `https://developers.openai.com/plugins/build/skills` | Provider authority | OpenAI skill behavior and packaging | Compatibility testing |
| `https://cursor.com/docs/plugins` | Provider authority | Agent Plugin and marketplace support | Future wrapper packaging |
| `https://code.claude.com/docs/en/plugins` | Provider authority | Claude plugin and skill layout | Future wrapper packaging |
| `https://geminicli.com/docs/extensions/` | Provider authority | Gemini extension and Agent Skill support | Future wrapper packaging |
| MCP marketplace release brief supplied 2026-09-02 | Owner-provided planning input | Activation event and distribution intent | Adapted; not treated as protocol authority |

## Synthesis decisions

| Decision | Status | Reason |
| --- | --- | --- |
| Classify as `workflow-process` | Adopted | The value is a guarded, repeatable activation workflow. |
| Use portable inline guidance | Adopted | One coherent path fits in one runtime file; references or scripts add no current runtime value. |
| Depend on one hosted MCP | Adopted | Account state and authorization belong in the service, not the skill. |
| Require accepted test status | Adopted | It matches the product activation event and prevents false completion claims. |
| Expose arbitrary recipient configuration | Deferred | Recipient verification and email side effects need a separate product review. |
| Add billing or destructive form tools | Rejected for v1 | They do not help the first activation event and expand risk. |
| Add provider-specific runtime instructions | Rejected | Client differences belong in manifests and installation docs. |

## Source adaptation

- Source intent: distribute html.contact across major coding-agent clients and
  complete one form plus one accepted test.
- Local target: a provider-neutral skill that orchestrates the live MCP and edits
  the user's site without carrying credentials or backend logic.
- Fidelity boundary: preserve the activation event, owner authorization, public
  form key rules, idempotency, confirmation, and accurate test status.
- Local replacements: replace the brief's broad tool inventory with the minimum
  activation workflow; use verified primary recipient instead of arbitrary routing.
- Omitted material: marketplace manifests, Stripe behavior, submissions browsing,
  deletion, Docker packaging, and server implementation.
- Rights: owner-supplied brief is summarized rather than copied; public links are
  used for authoritative platform and product behavior.

## Coverage

| Dimension | Status |
| --- | --- |
| Preconditions | Covered |
| Ordered flow | Covered |
| Failure handling | Covered |
| Safety boundaries | Covered |
| Expected output | Covered |
| Provider portability | Covered, pending client tests |

## Trigger examples

Should trigger:

- "Add a working contact form to this Astro site with html.contact."
- "Create a waitlist form, allow my production domain, and test it."
- "Wire this feedback form to my html.contact account."

Should not trigger:

- "Make this existing form look better on mobile."
- "Add client-side validation to our HubSpot form."
- "Build a Stripe payment form."
- "Explain what an HTML form action does."

## Open gaps and stopping rationale

- Replace planned tool names only after the production MCP contract is approved.
- Add client-specific forward-test results after each wrapper exists.
- Confirm the canonical public MCP docs URL before release.
- Further source collection is currently low-yield because the blocking facts are
  future product contracts and client tests, not missing documentation.
