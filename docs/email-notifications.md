---
title: Email Notifications & Safe Sender
description: Recognize html.contact form notifications, configure safe-sender rules, and understand Reply-To and delivery status.
---

html.contact stores an accepted submission before attempting its email notification. The submission in your dashboard is the durable record, even if a mailbox delays, filters, rejects, or suppresses the notification.

## Sender identity

Form notifications and form test emails use:

```txt
From: {Form name} via html.contact <forms@notify.html.contact>
```

For example, a form named `Website contact` appears as:

```txt
Website contact via html.contact <forms@notify.html.contact>
```

Renaming a form changes the visible name on later notifications. The exact sender address remains `forms@notify.html.contact`.

Do not trust a display name by itself. Display names can be imitated; use the complete sender address when creating a contact, filter, or allowlist rule.

## Add the sender to contacts or safe senders

For a personal mailbox, add this exact address to contacts or safe senders:

```txt
forms@notify.html.contact
```

For a managed business or school mailbox, an administrator may allow the sending subdomain:

```txt
notify.html.contact
```

Prefer an exact-address rule when your mail system supports it. Allowlisting can reduce filtering, but it cannot guarantee inbox placement; receiving providers still apply their own authentication, reputation, content, and policy checks.

## Reply-To behavior

Replying goes to the submitter only when html.contact inferred a valid Reply-To address from the submitted form. If no valid address was inferred, do not assume that replying will reach the submitter. Check the message's Reply-To header or open the stored submission before responding.

Submitted fields never control the From identity, To, CC, or BCC routing. Those values come from server-owned sender policy and verified form-recipient settings.

## If a notification is missing

1. Open the form submission in the html.contact dashboard. The dashboard record does not depend on email delivery.
2. Check the submission's email status and recipient configuration.
3. Search for mail from `forms@notify.html.contact`, including spam, quarantine, and filtered folders.
4. Confirm the primary recipient is verified and has not been paused after a delivery failure.
5. Add the exact address to contacts or safe senders, or ask your mail administrator about allowing `notify.html.contact`.

See [Troubleshooting](troubleshooting.md) for other submission and delivery checks.
