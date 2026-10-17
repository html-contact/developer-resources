---
title: Google Sheets
description: Save accepted form responses to a dedicated Google worksheet, with automatic columns and delivery history.
---

Google Sheets saves each new accepted response as one row. It is included on every plan. Availability is being introduced through a pilot; the Google Sheets card appears when your account is enabled.

Agency client workspaces do not support Google Sheets yet. Configure this integration only from **My forms**.

## Connect a response worksheet

1. Open **Forms → your form → Connections → Google Sheets**.
2. Choose **Connect Google** and authorize your Google account. It can differ from your html.contact login.
3. Choose **Choose spreadsheet**. Select an existing native Google spreadsheet with Google Picker. Pasting a spreadsheet URL does not grant permission.
4. Create a dedicated worksheet, select an empty worksheet, or return to a worksheet already managed for this form.
5. Review the Google account, spreadsheet, worksheet, and headers. Choose **Enable Google Sheets**.

Browsing and cancelling setup do not enable exports. Each form has one Sheets destination. One Google account connection can serve several forms; each form needs its own response worksheet. Spreadsheet creation, existing-column mapping, and historical exports are not included.

Google grants access to the spreadsheet files you select, including their worksheets. html.contact requests `drive.file`, `openid`, and `email`; it does not request access to all spreadsheets or all Drive files. Your selected-file permission is broader than the single response worksheet.

## Headers and response values

Every response includes **Submission ID**, **Submitted at (UTC)**, and **Submission URL**. The submission link requires signing in to html.contact.

If the form already has an eligible submission, setup uses its field names for the header preview. It does not export that response. Otherwise, custom columns appear with the first new response. New field names add columns on the right; missing fields leave empty cells. Removing a field retains its historical column. Renaming an HTML field creates a new column. System-header collisions receive a stable alternate label.

Values are literal text: formulas do not execute and leading zeros remain intact. Repeated values become JSON arrays. File fields contain attachment filenames; attachment bytes and public download links are not exported. A value over Google's cell limit fails visibly without truncation. A response worksheet can accumulate up to 500 custom columns.

You can format cells and rename the worksheet. Keep managed headers unchanged and in order. Put formulas, calculations, and other analysis on a separate worksheet. If headers are renamed, deleted, or reordered, syncing stops until you repair them and choose **Check connection**.

## Delivery and recovery

Only newly accepted real responses that pass spam checks are exported. Email-notification success is independent of Sheets delivery. Rejected, spam, and synthetic test submissions are excluded. Use **Check connection** to verify worksheet readiness.

The integration shows its last successful sync, pending count, oldest pending response, and delivery history. Temporary Google failures retry automatically. Eligible payloads remain available for up to seven days; safe delivery metadata remains for 30 days. Up to eight automatic attempts run within 24 hours of executable processing, excluding time blocked on a repair. Up to three manual attempts are available before expiry.

- **Reconnect required:** reconnect the same Google account. Eligible blocked responses resume after authorization is restored.
- **File permission denied:** restore editing access, then check the connection. Reconnecting alone cannot fix file-sharing permissions.
- **Missing worksheet or changed headers:** restore the worksheet or headers, then check the connection; alternatively choose a new destination.
- **Needs review:** Google did not confirm whether a write finished. Use **Recheck ID**. An uncertain resend requires acknowledgement because a duplicate remains possible if customer edits or a Google outage obscure the earlier row.
- **Cancelled or expired write:** review existing rows and acknowledge the recovery action. html.contact checks the saved schema before permitting future responses. Cancelled and expired responses are not resent.

Every row contains its submission ID. Ordinary duplicate queue messages do not append again, and uncertain writes are checked against that ID before another append. If duplicate IDs are found, review the rows in Google Sheets and acknowledge the discrepancy. html.contact does not delete customer rows.

An actionable outage also produces one email to the verified html.contact account owner, with a dashboard repair link and no submission content. Account-wide authorization failures are grouped. If email delivery cannot be confirmed, Settings shows that uncertainty and avoids automatically sending another copy.

## Pause, remove, or disconnect

**Pause** and **Remove integration** stop future exports and cancel pending exports after confirmation. Changing the destination also cancels outstanding work for the previous destination; those responses are never silently redirected. Removal retains safe delivery history and managed-worksheet ownership so the same form can reconnect it.

**Settings → Connections → Disconnect Google** stops dependent integrations and removes locally usable Google credentials. Use the link to Google's access settings to revoke the provider grant itself. Removing one form's integration does not disconnect the shared Google account.

Deleting or marking a submission as spam cancels unsent work. Rows already exported remain under your control in Google Sheets. A Google request already in flight may finish after a cancellation; recovery never resends that cancelled response. There is no two-way sync or propagation of later submission edits.
