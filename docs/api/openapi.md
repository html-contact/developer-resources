---
title: OpenAPI
description: Use the generated html.contact OpenAPI document as the exact Agency API contract.
---

The generated [OpenAPI JSON](https://html.contact/openapi.json) is the source of truth for public browser submission routes and the account-scoped Agency API.

It documents:

- Public `POST /f/{formKey}` and `POST /submit` routes using `hc_pub_` keys.
- Agency routes rooted at `/api/v1/accounts/{accountId}` using account-owned Agency API keys.
- Exact request, response, pagination, filter, revision, idempotency, and error schemas.
- Required service capabilities in `x-required-capabilities`.

It intentionally excludes `/api/app` dashboard routes, credential-management endpoints, billing management, recipient management, test email, form duplication/deletion, submission mutations, provider integrations, and MCP internals.

## Security schemes

Public form posts do not use bearer auth. Agency operations use:

```http
Authorization: Bearer hc_live_REPLACE
```

The scheme represents an Agency API key only. A browser session, OAuth/MCP token, or legacy personal key cannot substitute for it.

## Tooling

<p>
  <a href="/openapi.json" target="_blank" rel="noreferrer">Open raw OpenAPI JSON</a>
  <br />
  <a href="https://petstore.swagger.io/?url=https://html.contact/openapi.json" target="_blank" rel="noreferrer">View in Swagger UI</a>
</p>

Generated clients must preserve opaque cursors, `ETag`/`If-Match`, and `Idempotency-Key` semantics. They should also surface `X-Request-Id` and honor `Retry-After`.
