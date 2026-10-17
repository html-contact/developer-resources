---
title: Slack Notifications
description: Connect a Slack channel to a form, choose shared fields, and test notifications.
---

Send this form's accepted submissions to one Slack channel. Configure Slack in **Forms → your form → Connections**. Notifications are included on every plan. If connection setup is marked unavailable, Slack notifications have not been enabled for this environment yet.

Verified Owners and Admins can manage channels in the selected default or client workspace. Choose **Agency Forms** for your default workspace. In a client workspace, **Connections** lets an assigned Client contact choose an existing form and authorize their own Slack channel. Members cannot manage connections; they can see each form's connections and their health read-only. Client contacts retain read-only Forms and Submissions access and cannot open private Client Settings.

## Connect Slack

1. Choose **Connect Slack**. Select a channel you already authorized, or authorize a new one in Slack.
2. In Slack, select a workspace and the channel that should receive submissions. Your workspace may require administrator approval. For a private channel, you need permission to access it.
3. Back in html.contact, review the channel and notification preview. Choose all visitor fields, selected field names, or link only.
4. Save the channel. It starts paused. Optionally send a test, then choose **Enable integration**.

Authorization does not immediately send a message. Tests use clearly labeled synthetic data and your saved settings. They do not create submissions, consume submission usage, or email anyone. Notifications apply to future accepted submissions after you enable them.

“All form fields” includes fields added later. Slack messages show up to ten fields, with each value shortened to 500 characters, plus a link to the full submission. Attachments and internal metadata are excluded. Visitor values are rendered as plain text so they cannot trigger `@channel` mentions. Dashboard links require sign-in.

## Reuse or disconnect a channel

An authorized channel belongs to the selected html.contact workspace and can be reused across forms there. Client channels are separate from default-workspace channels and other clients. Each form has its own field selection, enabled state, tests, and delivery history.

**Settings → Connections** shows authorized channels and the forms using them in your selected default workspace. For a client, choose **Connections** in the selected workspace. Owners and Admins see every channel there with who manages it, plus each form's webhooks; **Client Settings → Connections** links to that page. Disconnecting stops those forms' notifications and cancels pending deliveries. It removes html.contact's local authorization; it does not uninstall the app from Slack. You can reconnect from a form. To remove the Slack installation itself, use your Slack workspace's app-management settings.

A Client contact can configure, test, retry and disconnect a channel they manage. They cannot overwrite another client contact's active channel or an Agency-managed channel. Each form has one Slack channel, so a Client contact cannot add their own channel to a form that already sends to the Agency's channel. An Owner/Admin who changes or removes a form's destination on a channel a Client contact manages confirms the takeover first. An Owner/Admin can explicitly hand a channel to an assigned Client contact after reviewing every affected form, or take it over through Slack authorization. A deliberately disconnected channel can be claimed by another assigned Client contact. This management change does not move the channel into another client workspace.

The **Connections** page also has a **History**: who authorized, authorized again, disconnected, handed over or took over each channel, and who added, changed, moved or removed each form's Slack notification. Owners and Admins see every change in the client workspace, and assigned Members see it read-only; a Client contact sees their own. The same changes appear for Owners, Admins and Members under **Activity → Connection changes**. Entries hold names, channel and form names and times, never messages or submission content, and are kept for 365 days.

The customer hub returns to the same client and chosen form after authorization, cancellation or failure. Saved destinations begin paused; **Enable integration** starts future accepted-submission notifications. Disconnecting a channel stops its pending work. Removing a contact's access, or removing a client from their access, asks the Owner or Admin whether to hand the channels they manage over to the Agency (they keep delivering) or disconnect them. Google Sheets and Zapier customer connections remain planned.

## Slack login is separate

**Continue with Slack** signs you into html.contact. **Connect Slack** authorizes notifications to a channel. Signing in does not authorize form notifications, and disconnecting a notification channel does not remove your login method.

You do not need to paste a Slack webhook URL, bot token, or signing secret. Slack's authorization flow supplies the channel destination to html.contact. Slack request-signing secrets verify inbound Slack requests; this notification-only integration does not receive Slack events or interactive commands.

## Troubleshooting

- **Authorization cancelled:** return to the form and connect again. Your current settings stay intact.
- **Authorize again:** Slack refused the channel (the app was removed, or the channel was archived or deleted). Every form sending to that channel stops, and its queued notifications are held for up to 7 days. The channel cannot be chosen or turned back on until it is authorized again. Authorize the same channel again from a form or the **Connections** page; html.contact then offers **Resume** for every form that stopped on it, which sends the held notifications. Owners and Admins resume Agency-managed channels. A channel a Client contact manages is resumed by that contact; an Owner or Admin who needs to resume it takes it over first.
- **Needs attention:** one form's notification stopped. Open **Manage**, fix the cause, then choose **Resume delivery**. Choosing another channel or **Save paused** cancels held notifications.
- **Queued or retrying:** Slack delivery respects channel rate limits and retries transient errors. Check **Recent deliveries → Details**.
- **No notifications:** confirm both the form and its integration are enabled. Rejected, spam, deleted and MCP synthetic submissions do not produce Slack notifications.

The [delivery and retry rules](webhooks.md#delivery-history-and-retries) also apply to Slack notifications. The app can be installed through its authorization flow; Slack Marketplace listing is a separate distribution process.
