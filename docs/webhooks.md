---
title: Signed Webhooks
description: Send form submissions to your endpoint, verify signatures, and manage delivery retries.
---

Webhooks send JSON to your app whenever this form accepts a real, non-spam submission. Configure up to three endpoints per form in **Forms → your form → Connections**. Webhooks are included on every plan. If setup is marked unavailable, webhook delivery has not been enabled for this environment yet.

Owners and Admins can manage webhooks in **Agency Forms** and Agency client workspaces. Select the client first, then open its form’s **Connections** tab. Members, client contacts, API keys and MCP cannot manage webhooks.

## Connect an endpoint

1. Choose **Add webhook**. Give it a name and enter a public HTTPS URL on port 443.
2. Choose **All form fields**, **Selected fields**, or **Link only**. The preview uses sample data. “All form fields” includes fields you add in the future.
3. Choose **Create webhook**. It starts paused. Reveal or copy its signing secret, install it in your receiver's server environment, and deploy or restart the receiver so it uses the secret.
4. Choose **Send test** to check your saved settings. This sends synthetic data without creating a submission, consuming submission usage, or sending an email.
5. Choose **Enable integration** when ready. A successful test is useful, but is not required to enable delivery. Save any edited settings before testing them.

Use exact HTML `name` attributes for selected fields, one per line. Repeated values are arrays. Attachments, control fields, IP addresses and internal security metadata are excluded. Dashboard links require sign-in. Keep signing secrets in server-side secret storage, never in your form HTML or browser JavaScript.

URLs containing credentials, IP literals, internal hostnames, fragments, or nonstandard ports are rejected. Redirects are not followed. Use the final public endpoint URL.

## Event body

[Version 1 JSON Schema](https://html.contact/schemas/webhook-v1.json)

```json
{
  "schema_version": 1,
  "id": "out_sub_example",
  "type": "submission.accepted",
  "timestamp": "2026-09-04T12:00:00.000Z",
  "data": {
    "form": { "id": "form_example", "name": "Website contact" },
    "submission": {
      "id": "sub_example",
      "fields": { "email": "casey@example.com", "message": "Hello!" }
    },
    "url": "https://html.contact/app/submissions/sub_example"
  }
}
```

Tests use `integration.test`, a synthetic submission ID, and a link to the form's Connections tab. New client events link to that client's dashboard; links require sign-in and current access. The maximum request body is 256 KiB. An oversized payload fails without sending; use selected fields or link-only mode if needed. An event's body is frozen before its first attempt and reused for retries. Treat visitor field values as untrusted input.

## Verify signatures before processing

Requests follow [Standard Webhooks](https://github.com/standard-webhooks/standard-webhooks/blob/main/spec/standard-webhooks.md). Headers are:

| Header | Meaning |
| --- | --- |
| `webhook-id` | Delivery ID for this endpoint, stable across retries |
| `webhook-timestamp` | Current attempt's Unix timestamp in seconds |
| `webhook-signature` | One or more space-separated `v1,<base64 signature>` values |

Remove the `whsec_` prefix and base64-decode the remaining secret into the HMAC key. Sign the exact bytes of `webhook-id + "." + webhook-timestamp + "." + raw request body` using HMAC-SHA256. Compare signatures in constant time. Accept any matching `v1` signature, and reject timestamps more than five minutes in the past or future. Keep your server clock synchronized.

Do not parse and reserialize the JSON before verification. Use a maintained Standard Webhooks verifier, or adapt the Node and Python reference implementations in the repository's `examples/webhooks/` directory. With the Node helper:

```js
import { verifyWebhook } from "./verify.mjs";

export async function POST(request) {
  const rawBody = new Uint8Array(await request.arrayBuffer());
  let event;
  try {
    event = verifyWebhook(rawBody, request.headers, process.env.WEBHOOK_SECRET);
  } catch {
    return new Response("Invalid signature", { status: 400 });
  }
  // Atomically store/enqueue the event with a unique constraint on its verified delivery ID.
  // A previously accepted ID should return 2xx without processing it again.
  await enqueueOnce(request.headers.get("webhook-id"), event);
  return new Response(null, { status: 204 });
}
```

`enqueueOnce` is your application's durable queue/database operation. Set a request-size limit in your HTTP server before reading the body. Acknowledge promptly after durable acceptance; process slower work asynchronously. The JSON `id` identifies the event; `webhook-id` identifies its delivery to this endpoint. One event can have different deliveries for different endpoints. Deduplicate by the verified `webhook-id` to preserve that distinction. Delivery is at least once and ordering is not guaranteed.

## Rotate a secret

Open **Manage → Rotate**. New requests carry signatures for both the new and previous keys for 24 hours. Update your receiver during that window. Another routine rotation is blocked until the overlap expires. **Compromised secret?** replaces it immediately and removes the previous key without an overlap.

Rotation does not resend events. Retries use the current signing keys and a fresh request timestamp, with the same delivery ID, event ID and raw body.

## Delivery history and retries

Recent deliveries show the newest 50 jobs for the form, including synthetic tests; **Load older deliveries** pages back through the rest of the 30-day history. **Details** shows individual attempts and safe status codes. “Delivered” means the receiver returned a successful HTTP response; it does not prove your downstream job completed.

Timeouts, network failures, HTTP 408, HTTP 429, and 5xx responses are retryable. Other responses end the automatic cycle. Redirects, authentication, missing-endpoint and revoked-destination errors may require attention; a failed test never changes the destination's status. There are up to eight automatic attempts within 24 hours. Scheduling uses backoff and respects `Retry-After`; recovery after a Queue interruption may wait for the ten-minute reconciliation cycle.

A failed delivery with a retained payload and unchanged, available destination can be retried manually up to three times. Each **Retry once**, available in its row or **Details**, requests one attempt. After an outage, **Retry failed…** sends one destination's failed deliveries created in the last hour, 24 hours or 7 days again, oldest first and up to 100 per request, each under the same rules as **Retry once**; repeat it for the rest.

Tests and manual retries have three limits, and each test, retry or **Retry failed…** request counts once toward all of them: five a minute and thirty an hour for each person, ten a minute and sixty an hour for each workspace (each client, and **Agency Forms** or **My Forms**), and thirty a minute and three hundred an hour for the whole account. A Client contact's Slack tests and retries count toward their own and their client's limits, never another client's. Automatic attempts never count, and integration tests and retries do not consume form submission usage.

If the destination shows **Needs attention**, delivery has stopped. Deliveries already queued are held, not cancelled, until their payload expires; submissions accepted while it needs attention are not queued. Open **Manage** and inspect the failed delivery. If you fixed the receiver without changing the integration's settings, choose **Resume delivery**: held deliveries are sent and the failed delivery can be retried. If you change the endpoint or settings, or choose **Save paused**, the new revision cancels held deliveries; send a new test, then enable the integration. Older deliveries cannot be retried against that changed configuration.

Payload snapshots are encrypted and retained for seven days. Delivery metadata is retained for thirty days. Pausing or changing a destination cancels pending deliveries; it does not replay old submissions. Pausing/deleting a form, deleting or marking a submission as spam, or disconnecting a Slack grant prevents later delivery. A request already accepted by a receiver cannot be recalled. Removing a destination removes its delivery history.

Removing a teammate immediately removes their management access. Existing webhooks and already committed tests or retries remain active for the workspace. Archiving a client stops later delivery; restoring it leaves forms paused and does not replay cancelled work.

## Connection history

Every webhook change is recorded in the workspace's connection history: who added, changed, pointed elsewhere, paused, turned on or removed a webhook, and who revealed or rotated its signing secret, with the form, the endpoint host (and the previous host after a URL change) and the time. Owners, Admins and Members see it under **Activity → Connection changes**, and for a client also on its **Connections** page under **History**. The history never contains URLs, secrets or submission content, and is kept for 365 days.
