---
title: Agency Clients
description: Manage client workspaces, teammate access, client contacts, and workspace settings.
---

Agency mode is in a pilot and is enabled on request; [contact us](https://html.contact/contact/) to ask about access. When the self-serve Agency plan becomes available, Owners and Admins can choose it on the account's **Billing** page. When Agency mode is enabled for your account, the dashboard adds a client selector. **Agency Forms** is your agency's original workspace, separate from each client's forms, submissions, activity, and recipient settings. Non-Agency accounts use **My Forms**. Agency pilot access does not mean Agency is included in the ordinary Free, Starter, or Pro plans.

## Teammates and client contacts

Creating a client creates a workspace, not a login or invitation. Owners and Admins manage invitations in **Agency settings → Teammates** for internal staff and **Agency settings → Client contacts** for external portal access. Each list can be searched, filtered and paged; **Edit access** opens a panel where you choose workspaces (up to 500 per person) and confirm the result before it is saved.

| Role | Access |
| --- | --- |
| Owner | Account-wide management, including ownership transfer. |
| Admin | Account-wide management, except ownership transfer. |
| Member | Work in assigned workspaces, including notification recipients, linked emails and test emails, without account administration. Only Owners and Admins can permanently delete forms or submissions. **All clients** includes current and future client workspaces, not Agency Forms unless separately assigned. |
| Client | Read-only Forms and Submissions in explicitly assigned client workspaces, plus own Slack channel controls in Connections. No Agency administration, private settings, form editing or submission mutations. |

An invitation must be accepted while signed in with its matching verified email address, and it opens the account you joined. Owners and Admins can resend or revoke pending invitations and update or remove non-owner access; each access change is confirmed with its result first. Client contacts and teammates are managed separately: to move someone between them, remove their access and send a new invitation. Removing a teammate also revokes the invitations they sent. Ownership changes require the Owner's separate transfer action. If you belong to more than one account, switch between them from the account menu at the bottom left. Receiving a form notification or confirming a linked email never grants dashboard access.

The hosted Client portal can display your agency's name and logo, configured under **Portal branding**. Custom portal domains are not supported. **Access history** shows up to 180 days of user-facing access events; Owners and Admins can view account history, while Members and client contacts can view their own history.

## Create and select a client

Open **Agency overview**, then choose **Create client**. Creation records only the client name and workspace. It does not create a login, form, recipient, provider connection, or email.

Choose a client from the selector at the top left, or press Cmd+K (Ctrl+K on Windows and Linux) from anywhere in the dashboard. The switcher lists Agency pages, your recent clients, all clients and archived clients, and keeps you on the same page in the new client where it exists. The selected client name remains visible while you work. A client may stay empty, and its forms may collect submissions with notification email off and no recipient.

## Find the right settings

- **Personal settings:** your profile, sign-in/security, and your own access history.
- **Account settings:** billing and account finance controls, available to Owners and Admins.
- **Agency settings:** teammates, client contacts, portal branding, Agency API keys, and account access history, available to Owners and Admins.
- **Connections:** choose an existing form and authorize Slack in the selected client workspace. Client contacts control their own channels; Owner/Admin can explicitly hand over an Agency-managed channel after reviewing affected forms. An active channel managed by another contact cannot be taken over by a Client contact. After a channel is authorized again, Owners and Admins resume the forms on Agency-managed channels; forms on a channel a Client contact manages are resumed by that contact, or by an Owner or Admin after taking the channel over. **History** on the same page lists who connected, changed, handed over, took over or removed each connection; Client contacts see only their own changes. Owners, Admins and Members also see these changes under **Activity → Connection changes**. Connection history is kept for 365 days.
- **Client settings:** private client details and notes; Owners and Admins can also manage that client's linked emails and client contacts. Assigned Members can read details, edit notes and manage linked emails, but cannot change contact details or manage portal access. Client contacts cannot read these internal details or notes.

Saving a primary contact name or email under **Details** does not invite anyone, verify an address, set a form recipient, or send email. To give portal access, invite a **Client contact**. To route notification email, configure a **Linked email** and select it on the form.

Agency API keys can be restricted to specific workspaces or cover the entire Agency account; see the [Agency API](api.md) and [key creation and safety](api/authentication.md). Owners and Admins can configure [signed webhooks](webhooks.md) and [Slack notifications](slack.md) on client forms. Client Slack channels stay separate from other clients and the default workspace. MCP, Zapier, Google Sheets, and other provider connections remain limited to the original workspace (**Agency Forms**, or **My Forms** for non-Agency accounts). Those providers do not yet support Agency client workspaces.

In **Agency Forms**, the Zapier connection (a pilot) belongs to the person who connected it, the account's original Owner. Other Owners and Admins see it under **Settings → Connections → Zapier**, with the forms it reads and how many Zaps each has, and can disconnect it. It keeps working while that person is an Owner or Admin, including after they transfer ownership; it pauses if they become a Member and is disconnected when their access is removed. Not yet supported: a new Owner connecting their own Zapier account to Agency Forms, and forms that an Admin or Member created in Agency Forms (only the original Owner's forms can be approved for Zapier).

## Usage, caps, and billing

Every client's accepted submissions draw on one allowance for the account. **Agency overview** and **Billing** show how much of the current billing period's allowance is used and what each client used.

- **Client caps:** on **Billing → Client usage**, Owners and Admins (who can also change the subscription) can set a monthly cap for any client. When a client reaches its cap, that client's forms return the over-limit response until the period resets or the cap changes. Other clients keep working.
- **Alerts:** on a paid plan, Owners and Admins get an email when the account reaches 80% and 100% of its allowance, and when a client reaches its cap. Each alert is sent once per billing period to each of them and contains no submission content. Free accounts do not get usage alert emails.
- **Monthly or yearly:** switching to yearly billing in the billing portal starts a new billing period, and with it a new usage period, from that day. So does switching to another plan with a different billing interval.
- **Billing contact:** billing belongs to the account, and the Owner is its billing contact. Transferring ownership moves billing email and the Stripe customer contact to the new Owner. The card on file keeps paying renewals until the new Owner replaces it: Billing shows a notice until renewals are moved to a payment method the new Owner added with **Replace payment method**. A card added any other way, for example by an Admin in the billing portal, does not end the notice. If a plan change is scheduled for the end of the billing period, contact support to move that change to the new card too. Only the Owner can replace the card, with **Replace payment method**; Admins see the notice too. Removing the previous card holder from the account warns that renewals still charge their card.
- **When Agency access ends:** client workspaces keep working for a 7-day grace period, and the account's Owners and Admins get one email when it starts, with the date access ends. After it, Owners and Admins can still open every client read-only and download its hand-off bundle, as with an archived client. They can also still delete data permanently: a whole client (no need to archive it first), a form, or Spam submissions, with the usual confirmations. Nothing else in a client can be changed and no one can be invited. Client caps cannot be set or changed, but an existing cap still applies until you remove it on **Billing → Client usage** (removing a cap stays possible). Teammates and client contacts can no longer open client workspaces, and pending invitations cannot be accepted. Agency API keys stop working but are not deleted: they work again when Agency access returns, so revoke any key you no longer trust in **Agency settings → API keys** before then; new keys can be created only with Agency access. Client data is kept until you delete it: clients stay listed on **Agency overview**, and full access returns when Agency access does. When Agency access ends, every client's Slack, Google Sheets, webhook and Zapier connections are disconnected and their saved credentials are deleted (tokens, Slack channel links, webhook URLs and signing secrets; Google Sheets and Zapier are pilots). You will need to reconnect them after subscribing again.

## Archive and restore

Archiving a client stops new public intake and pauses its active forms. Existing forms, submissions, settings, and activity are preserved; archiving is not deletion. An archived client is read-only: Owners and Admins can still open it to read and export its forms and submissions, but nothing in it can be changed. Members and client contacts lose access to it.

Archiving also disconnects the client's Google, Slack, webhook and Zapier connections and deletes their saved credentials (tokens, Slack channel links, webhook URLs and signing secrets). Delivery history is kept. A disconnected webhook cannot be turned back on; remove it and add a new one after a restore. To revoke html.contact's access at the provider as well, remove it in your Google or Slack account settings.

Restoring returns access to the workspace. Its forms remain paused until you review and reactivate them, and its connections stay disconnected until you reconnect them. Restoration does not replay cancelled or skipped delivery work and does not send email. A provider send already issued before archive cannot be recalled.

Owners and Admins manage client archive and restore. The original **Agency Forms** workspace cannot be archived through these controls.

## Hand off or delete a client

When a client leaves, an Owner or Admin can choose **Download hand-off bundle** from the client's menu on the Agency overview. The ZIP contains a CSV of every form with its public endpoint and allowed domains, a CSV of each form's submissions, a list of uploaded files (the files themselves are downloaded from each submission or with an Agency API key), and instructions for moving the forms. A very large client's bundle stops at a size limit and its instructions say so; export the rest from each form's Submissions page. Forms cannot be moved between html.contact accounts yet: the client creates their own forms and changes the form action URLs on their website.

To erase a client, archive it first, then choose **Delete client** (once Agency access has ended, any client can be deleted this way without archiving it first). Owners and Admins see exactly what will be deleted before confirming, type the client's name, and need to have signed in within the last 15 minutes. Deletion removes the client's submissions and uploaded files, forms, recipients and connections, notification emails, details and notes, activity, and teammate and client contact assignments. It cannot be undone. Billing usage totals, Agency API key history, access history until it expires, and a record that the client was deleted are kept. Large clients are deleted in steps; deletion continues in the background if you leave the page.

Deleting your whole Agency account is not available in the dashboard. To request it, email support@html.contact.
