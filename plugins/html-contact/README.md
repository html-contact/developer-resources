# html.contact plugin

Publisher: 854 Labs

Package version: `0.1.0`

Tested MCP contract: production `1.5.0`, twenty tools

This package connects the hosted html.contact MCP app and adds a portable operating skill for safe form setup, diagnosis, installation, testing, recipient routing, and bounded submission review. It contains no OAuth token, client secret, reviewer credential, private account identifier, or customer data.

## Install for local review

From a checkout of this repository:

```bash
codex plugin marketplace add /absolute/path/to/developer-resources
codex plugin add html-contact@html-contact-developer-resources
```

After the package is published on `main`, the marketplace can instead be added from GitHub:

```bash
codex plugin marketplace add html-contact/developer-resources --ref main
codex plugin add html-contact@html-contact-developer-resources
```

Start a fresh Codex task after installation so the plugin skill and registered app are loaded into the new task.

## Authorization

Register a new client with `forms:read` as its default product scope where the host supports it. Request `forms:write`, `submissions:read`, `recipients:read`, or `recipients:write` when needed and not already granted. The server publishes the required OAuth scope on every tool; an existing grant does not need to be repeated for each action.

For the single-owner private ChatGPT preview, narrow base consent and denial were verified before the owner intentionally accepted all advertised product scopes. ChatGPT's observed incremental reconnect requested all product scopes. That private choice does not reduce tool capabilities, bypass ownership checks, or replace confirmation for mutations; it is not public-review least-privilege evidence.

Never paste an OAuth value or `hc_live_` key into a prompt. Use the hosted account-link flow.

## Compatibility evidence

| Host | Package | Server | Registration and callback | Verified scope/evidence | Status |
| --- | --- | --- | --- | --- | --- |
| ChatGPT developer mode | `0.1.0` | `1.5.0` | DCR; exact hosted ChatGPT HTTPS callback | Existing-owner OAuth, denial, narrow base consent, intentional full private grant, twenty-tool discovery, bounded form/linked-email-count/submission-metadata reads | Private read baseline pass; controlled writes, selected synthetic detail, lifecycle, and reviewer gates remain |
| Codex CLI `0.147.0` | direct MCP predecessor | `1.5.0` | DCR; narrowly constrained native loopback | Saved-credential form-list and recipient-scoped linked-email reads | Read baseline pass |
| MCP Inspector | direct MCP predecessor | `1.4.0` historical contract | DCR; exact `/oauth/callback` path | Exact fifteen-tool discovery and token revocation | Historical baseline pass |

A valid manifest is not cross-client certification. Add a row only after that host's actual registration, callback, OAuth, tool-discovery, positive/negative selection, and disconnect behavior are observed.

## Review boundary

This is a private release candidate. Do not claim public directory approval or remove html.contact's account allowlist. Production writes require a designated disposable fixture and the exact confirmation defined in the skill. Public review also requires a least-privilege connection, the complete lifecycle matrix, reviewer account, accurate privacy disclosure, and manual ChatGPT logo upload.
