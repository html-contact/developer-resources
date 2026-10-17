---
title: Agency API Examples
description: Copy trusted server examples for the account-scoped Agency API.
---

Create the API key in **Agency settings → API keys**. Your account ID starts with `ba_` and appears in the dashboard address after `/app/accounts/` when you open an account settings page such as **Billing**. Client, form, submission, and attachment IDs come from API responses.

```bash
export HC_SERVICE_CREDENTIAL="hc_live_REPLACE"
export ACCOUNT_ID="account_REPLACE"
export CLIENT_ID="client_REPLACE"
export FORM_ID="form_REPLACE"
export SUBMISSION_ID="sub_REPLACE"
export ATTACHMENT_ID="att_REPLACE"
```

## List clients

```bash
curl "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients?limit=50" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL"
```

Use the returned `nextCursor` unchanged for the next page.

## Create a client

Requires an all-client credential with `clients:create`.

```bash
curl -i "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: create-acme-2026-09-12" \
  -d '{"name":"Acme"}'
```

## Rename a client safely

First read the client and copy its `ETag` response header. Then:

```bash
curl -i -X PATCH "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL" \
  -H "Content-Type: application/json" \
  -H 'If-Match: "REVISION_FROM_READ"' \
  -H "Idempotency-Key: rename-acme-2026-09-12" \
  -d '{"name":"Acme Studio"}'
```

Archive and restore use the same two headers at `/archive` and `/restore`. Archiving is reversible and pauses active forms; restoring a client does not reactivate those forms.

## Create and activate a form

Creation produces a recipient-free draft with notifications disabled.

```bash
curl -i "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID/forms" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL" \
  -H "Content-Type: application/json" \
  -H "Idempotency-Key: create-contact-form-2026-09-12" \
  -d '{"name":"Website contact"}'
```

Use `PATCH .../forms/$FORM_ID` for the bounded settings schema. Use `POST .../forms/$FORM_ID/activate` or `/pause` with current `If-Match` and a unique idempotency key. Recipient routing is configured in the signed-in dashboard, not this API.

## List and read submissions

```bash
curl "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID/submissions?status=complete&limit=50" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL"

curl "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID/submissions/$SUBMISSION_ID" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL"
```

The list is content-free. Detail returns bounded submitted fields and attachment metadata; treat visitor data as untrusted.

## Export CSV and download an attachment

```bash
curl -L "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID/submissions.csv?formId=$FORM_ID" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL" \
  -o submissions.csv

curl -L "https://html.contact/api/v1/accounts/$ACCOUNT_ID/clients/$CLIENT_ID/submissions/$SUBMISSION_ID/attachments/$ATTACHMENT_ID" \
  -H "Authorization: Bearer $HC_SERVICE_CREDENTIAL" \
  -o attachment.bin
```

CSV export is capped at 5,000 rows and neutralizes spreadsheet formulas. Attachment IDs come from detail responses. Add `?disposition=inline` only for an intentional preview; the server permits it only for safe media types.

See the generated [OpenAPI document](https://html.contact/openapi.json) for exact bodies, filters, capabilities, and error schemas.
