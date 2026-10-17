---
title: Agency API Authentication
description: Create and safely use Full or Restricted Agency API keys.
---

Agency API calls use an account-owned API key created in **Agency settings**.

Only an Agency **Owner** or **Admin** can create keys, and creation requires a
recent sign-in. Internal **Members** and external **Client** contacts cannot
create or manage Agency API keys. Personal API-key creation is retired.

```http
Authorization: Bearer hc_live_REPLACE
```

The full secret is shown once. Store it in a server-side secret manager and revoke it when the automation no longer needs access. Keys can have no expiration or an optional expiry up to one year away. Revocation and expiry take effect on the next request.

## Create and save a key

1. Open **Agency settings → API keys** and name the key for its intended automation.
2. Prefer **Restricted access**: select only the required workspaces and capabilities. Choose an expiration appropriate for the task.
3. Create the key. If prompted, sign in again to satisfy the recent-authentication requirement.
4. In **Save your key**, check the issued permissions, workspace access, and expiration. Copy the secret directly into your secret manager before choosing **Done**.

Closing the dialog, including with Escape, dismisses the secret. The key inventory cannot reveal it later. If it is lost, create a replacement and revoke the old key. Never include the secret in screenshots or support messages.

## Full and Restricted access

- **Full access:** every current Agency API capability across Agency Forms and all current and future clients.
- **Restricted access:** an explicit `fixed` workspace list or `all_clients` account boundary plus selected capabilities. Each operation requires the capability listed as `x-required-capabilities` in OpenAPI.

`clients:create` and `account:usage:read` require `all_clients`. A key restricted to specific workspaces cannot acquire them. Neither key type can manage billing, team access, account deletion, or API keys. A request is also denied when the account, client, key, or underlying grant is no longer active.

Agency API keys, browser sessions, OAuth/MCP tokens, and legacy personal API keys are separate authorities. They are not interchangeable even when a legacy key shares the `hc_live_` prefix.

## Status codes

| Status | Meaning |
| --- | --- |
| `401` | Missing, malformed, unknown, revoked, or expired Agency API key. |
| `403` | Valid credential without the required capability for an otherwise visible resource. |
| `404` | Resource is absent or deliberately concealed outside the authorized tenant. |
| `409` | Idempotency key conflicts with a different request, or the matching request is still in progress. `recipient_not_ready` means the form's To recipient is not verified or cannot receive email; fix it in the dashboard instead of retrying. |
| `412` | A valid `If-Match` revision is stale. A revision token bound to another credential or resource is malformed for this request and returns `400`. |
| `428` | A required `If-Match` header is missing. |
| `429` | Rate limit reached; honor `Retry-After` when present. |

Never ask someone to paste an API key into a chat prompt. Rotate by creating a replacement, updating the trusted runtime, verifying it, and revoking the old key.
