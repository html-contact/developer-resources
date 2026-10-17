---
title: Zapier
description: Connect selected html.contact forms to Zapier with an instant accepted-submission trigger.
---

The native html.contact Zapier app is being tested in an invited pilot. Contact [support@html.contact](mailto:support@html.contact) for pilot access. This page does not imply a public App Directory listing.

Agency client workspaces are not available to Zapier yet. Zapier access remains limited to forms in **My Forms** (or **Agency Forms** in an Agency account).

The **New Accepted Submission** instant trigger starts a Zap after a selected form accepts a real, non-spam submission. It includes submitted fields, submission information, a dashboard link, and an optional attachment. It is included on every html.contact plan; your Zapier plan’s limits still apply.

## Connect a form

1. Open your pilot invitation in Zapier and create a Zap with **html.contact → New Accepted Submission**.
2. Connect your account. Sign in to html.contact, select the forms Zapier may access, and approve them. You do not need a personal API key.
3. Choose a form in the trigger. Only approved, active forms appear.
4. Submit a real example through your website’s form, then test the trigger. Up to three recent accepted submissions are returned. An empty form has no real sample; submit an example and test again. Zapier may display an illustrative static example while configuring the app.
5. Map the fields into a destination action and test that action. Turn on the Zap.
6. Submit a new example after activation and check Zap History for the complete result.

Old submissions are available as editor samples but are not imported when a Zap is enabled. Dashboard test notifications and agent-generated synthetic submissions do not start Zaps. Each form supports ten active Zapier subscriptions, separate from its three generic webhook slots.

## Map fields and attachments

**Submission ID** is the stable html.contact submission identifier. Standard output also includes Form ID, Form Name, Submitted At, and Submission Link. Custom fields use their original names as labels and separate stable mapping keys, so a visitor field named `id` cannot overwrite Submission ID. Repeated values and empty strings are preserved. **Original Fields JSON** includes every submitted custom field, including new fields absent from your editor samples. Refresh the trigger sample after changing your website’s field names.

For a file action, map **Attachment File** to the destination’s file input. The file downloads only when Zapier needs it. The current limit is one attachment up to 4 MB. Filename, type, size, and attachment ID are also available. Forms without files return an empty Attachment File value; use a Zapier Filter or conditional workflow before an action that requires a file.

Zapier and the receiving app keep their own downloaded copies. Revoking access in html.contact cannot recall those copies. A deleted submission or missing object cannot be downloaded again; select a fresh sample if an old sample’s file has been deleted.

## Delivery and duplicates

Delivery is **at least once**. Zapier’s instant triggers do not deduplicate incoming requests. If Zapier accepts a notification but its acknowledgement is lost, a retry can create another Zap run, even though the Submission ID and delivery ID remain the same. See [Zapier’s duplicate handling](https://help.zapier.com/hc/en-us/articles/8496260269965-How-Zapier-handles-duplicate-data-in-Zap-workflows).

Where your destination supports an upsert, use Submission ID as its unique key. For example, configure a CRM’s “Create or Update Record” action to match a custom `html_contact_submission_id` field. A simple “Create Row” action may append duplicate rows during retries. A separate search-then-create sequence is not atomic and may still race.

html.contact makes up to eight automatic delivery attempts within 24 hours, honors retry delays, retains delivery payloads for seven days, and retains safe delivery history for thirty days. “Delivered” in html.contact means Zapier accepted the notification. Check **Zap History** to confirm the downstream action completed.

## Manage access and recover

All Zaps connected to one html.contact account share one set of approved forms. New forms are never automatically approved. Go to **Settings → Connections → Zapier** to change the selection. Removing a form disconnects its subscriptions and cancels pending deliveries. Reapproving it does not restore cancelled work: re-enable the affected Zap in Zapier.

Each form’s **Connections** tab lists its Zapier subscriptions and recent deliveries. **Disconnect Zap** cancels that subscription. **Disconnect Zapier** in Settings revokes account access and cancels every dependent subscription. Downloaded data remains in downstream apps.

In an Agency account, the Zapier connection for **Agency Forms** belongs to the person who connected it (the original Owner). Every other Owner and Admin sees it in **Settings → Connections → Zapier**, with the forms it reads and their Zap counts, and can disconnect it. It pauses while its holder is not an Owner or Admin, resumes if they become one again, and is disconnected when their access is removed. Connecting, changing the approved forms and disconnecting appear in **Activity → Connection changes**. A new Owner cannot yet connect their own Zapier account to Agency Forms, and forms created by an Admin or Member there cannot be approved.

| Problem | Recovery |
| --- | --- |
| No forms in the dropdown | Approve the form in Settings, confirm it is active, then reload the Zapier dropdown. |
| No sample records | Submit a real example through the website and test the trigger again. |
| Authorization changed in another window | Restart the connection from Zapier. Another consent or Settings change took precedence. |
| Expired or revoked connection | Reconnect html.contact in Zapier and review the selected forms. |
| Subscription stopped or removed | Check Zap History, reconnect if necessary, and turn the Zap off and on to create a new subscription. A subscription that stopped because Zapier refused it (**Needs attention**) is replaced by the new one. |
| Attachment unavailable | Check the submission still exists, the file is still stored, and its form remains approved. |
| Delivery retrying | Check the scheduled attempt in html.contact. Avoid manual retries unless duplicate downstream work is acceptable. |
| html.contact says Delivered, but no destination record | Open the Zap’s history in Zapier and inspect the action step. Transport acceptance does not prove action success. |

For support, send the submission ID, delivery ID, approximate time, and safe error code to [support@html.contact](mailto:support@html.contact). Never send OAuth tokens or complete hook URLs.
