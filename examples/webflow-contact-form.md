---
title: Webflow Contact Form
description: Connect a Webflow form to html.contact with a public form endpoint.
---

Webflow can post normal form data. html.contact gives that form a backend, email delivery, attachments, and account logs.

## Setup

1. Create a form in html.contact.
2. Copy the public endpoint that starts with `https://html.contact/f/hc_pub_`.
3. In Webflow, set the form method to `POST`.
4. Set the form action to the html.contact endpoint.
5. Make sure every field you want stored has a field name.
6. Add the exact Webflow or custom hostname that serves the form to html.contact allowed domains.

## Example fields

Use names like:

```txt
name
email
phone
company
subject
message
attachment
```

The submitted field named `subject` is stored, can appear in the dashboard table, and can control the notification subject.

html.contact stores submitted values by field name. For dropdowns, checkboxes, and radio buttons, the submitted option value is what gets stored under that field name.

Recipients are configured inside html.contact, not with hidden Webflow fields.

## Attachments

If your Webflow form includes a file upload, the form needs to submit as `multipart/form-data`. html.contact supports one attachment up to 4 MB per submission.

Email notifications link to authenticated attachment downloads. In the dashboard, open the submission, find **Attachments**, then use **File** to view or download the upload. Scan unknown downloads with your security tools before opening them.

For the full HTML reference, see [Simple HTML Contact Form](/examples/simple-html-contact-form/).
