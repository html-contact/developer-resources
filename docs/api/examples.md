---
title: Private API Examples
description: Copy trusted server examples for html.contact private API keys.
---

These examples use the production base URL:

```txt
https://html.contact
```

Use a private API key that starts with `hc_live_`. Keep private keys on trusted servers, jobs, scripts, and approved agents only.

For browser forms that use public `hc_pub_` keys, see [HTML Forms](/docs/html-forms/) and [Examples](/examples/).

## Set example values

Set these variables before running the examples. Replace every value ending in `_REPLACE` with a real value from your account.

```bash
export HC_API_KEY="hc_live_REPLACE"
export FORM_ID="form_REPLACE"
export SUBMISSION_ID="sub_REPLACE"
export ATTACHMENT_ID="att_REPLACE"
```

| Variable | Replace with |
| --- | --- |
| `HC_API_KEY` | A private API key from your account |
| `FORM_ID` | A form ID returned by the Forms API |
| `SUBMISSION_ID` | A submission ID returned by a submission list or detail response |
| `ATTACHMENT_ID` | An attachment ID from the submission detail response |

## List forms

Requires a Read only or Form manager key.

```bash
curl "https://html.contact/api/v1/forms" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Response shape:

```json
{
  "ok": true,
  "forms": [
    {
      "id": "form_REPLACE",
      "publicKey": "hc_pub_REPLACE",
      "name": "Website contact",
      "status": "active",
      "recipientEmail": "owner@example.com",
      "recipientVerifiedAt": "2026-06-15T18:04:12.000Z",
      "allowedDomains": ["example.com"],
      "defaultSubject": "New website inquiry",
      "defaultIntro": "A new message arrived from your website.",
      "defaultRedirectUrl": "https://example.com/thanks",
      "emailNotificationsEnabled": true,
      "allowDirectPosts": false,
      "honeypotField": "_gotcha",
      "lifetimeSubmissionCount": 7,
      "lifetimeSubmissionLimit": 250,
      "createdAt": "2026-06-15T17:50:30.000Z",
      "updatedAt": "2026-06-15T18:04:12.000Z",
      "formRecipients": [
        {
          "email": "owner@example.com",
          "role": "to",
          "verificationStatus": "verified",
          "deliverabilityStatus": "ok",
          "verifiedAt": "2026-06-15T18:04:12.000Z"
        }
      ],
      "snippet": "<form ...></form>"
    }
  ]
}
```

Each form includes its ID, public key, name, status, allowed domains, recipient settings, and generated HTML snippet.

## Create a form

Requires a Form manager key.

```bash
curl "https://html.contact/api/v1/forms" \
  -H "Authorization: Bearer $HC_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Website contact",
    "recipientEmail": "owner@example.com",
    "ccEmail": "ops@example.com",
    "bccEmail": null,
    "allowedDomain": "example.com",
    "defaultSubject": "New website inquiry",
    "defaultIntro": "A new message arrived from your website.",
    "defaultRedirectUrl": "https://example.com/thanks",
    "emailNotificationsEnabled": true
  }'
```

`recipientEmail`, `ccEmail`, and `bccEmail` must already be verified recipient settings on the account.

## Update a form

Requires a Form manager key.

```bash
curl -X PATCH "https://html.contact/api/v1/forms/$FORM_ID" \
  -H "Authorization: Bearer $HC_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Main contact form",
    "allowedDomains": ["example.com", "www.example.com"],
    "allowDirectPosts": false,
    "emailNotificationsEnabled": true,
    "status": "active"
  }'
```

`allowDirectPosts: true` accepts trusted server or curl posts without browser source headers. Leave it off for normal website forms.

Set `emailNotificationsEnabled` to `false` when you want accepted submissions stored without notification email attempts.

## Duplicate a form

Requires a Form manager key.

```bash
curl -X POST "https://html.contact/api/v1/forms/$FORM_ID/duplicate" \
  -H "Authorization: Bearer $HC_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Website contact backup"
  }'
```

Duplicating copies recipients, allowed domains, defaults, and generated snippet behavior from the source form.

## Send a test email

Requires a Form manager key.

```bash
curl -X POST "https://html.contact/api/v1/forms/$FORM_ID/test-email" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Test emails send to the verified primary recipient and any verified, healthy CC/BCC recipients.

## Send recipient verification

Requires a Form manager key.

```bash
curl -X POST "https://html.contact/api/v1/forms/$FORM_ID/verify-recipient" \
  -H "Authorization: Bearer $HC_API_KEY"
```

If the action is rate-limited, the response includes `retry_after_seconds`.

## List submissions

Requires a Read only or Form manager key.

```bash
curl "https://html.contact/api/v1/forms/$FORM_ID/submissions?folder=inbox&limit=50" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Workspace-wide submissions:

```bash
curl "https://html.contact/api/v1/forms/all/submissions?folder=spam&q=casino" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Response shape:

```json
{
  "ok": true,
  "submissions": [],
  "next_cursor": null,
  "counts": {
    "inbox": 0,
    "spam": 0,
    "archived": 0
  }
}
```

Use `next_cursor` as the `cursor` query parameter to load the next page.

## Export CSV

Requires a Read only or Form manager key.

```bash
curl -L "https://html.contact/api/v1/forms/all/submissions.csv?folder=inbox&limit=5000" \
  -H "Authorization: Bearer $HC_API_KEY" \
  -o submissions.csv
```

CSV exports include attachment count, filenames, sizes, and authenticated download links when attachments exist.

## Get submission detail

Requires a Read only or Form manager key.

```bash
curl "https://html.contact/api/v1/submissions/$SUBMISSION_ID" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Detail responses include submitted field data, source metadata, email status, events, and attachment metadata.

## Download an attachment

Requires a Read only or Form manager key.

```bash
curl -L "https://html.contact/api/v1/submissions/$SUBMISSION_ID/attachments/$ATTACHMENT_ID" \
  -H "Authorization: Bearer $HC_API_KEY" \
  -o attachment.bin
```

For safe previewable file types, request inline preview:

```bash
curl -L "https://html.contact/api/v1/submissions/$SUBMISSION_ID/attachments/$ATTACHMENT_ID?disposition=inline" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Attachment metadata is kept separately from the file content, and files are downloaded through authenticated attachment endpoints.

## Get usage summary

Requires a Read only or Form manager key.

```bash
curl "https://html.contact/api/v1/usage" \
  -H "Authorization: Bearer $HC_API_KEY"
```

Response shape:

```json
{
  "ok": true,
  "usage": {
    "forms_count": 3,
    "submissions_count": 187,
    "used": 187,
    "limit": 1000,
    "plan": "starter",
    "plan_label": "Starter",
    "period_end": "2026-07-17T00:00:00.000Z",
    "is_over_limit": false
  }
}
```

## OpenAPI

<p>
  <a href="/openapi.json" target="_blank" rel="noreferrer">Open raw OpenAPI JSON</a>
  <br>
  <a href="https://petstore.swagger.io/?url=https://html.contact/openapi.json" target="_blank" rel="noreferrer">View in Swagger UI</a>
</p>
