---
title: Attachments
description: Accept files from normal HTML file inputs and review them from your account.
---

html.contact supports file uploads from normal HTML file inputs.

## Form setup

Use `enctype="multipart/form-data"` when your form includes files. Keep the public `hc_pub_` key in the form action.

```html
<form
  action="https://html.contact/f/hc_pub_REPLACE"
  method="POST"
  enctype="multipart/form-data"
>
  <label for="email">Email</label>
  <input id="email" name="email" type="email" required>

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <label for="attachment">Attachment</label>
  <input id="attachment" name="attachment" type="file">

  <button type="submit">Send</button>
</form>
```

## Private files

Uploaded files are stored privately. Each submission includes attachment metadata such as filename, size, field name, and file type. The file input still needs a `name` attribute; the uploaded filename is stored with the submitted fields under that name.

## Email notifications

Email notifications link to authenticated downloads instead of attaching raw files directly to outbound email.

## Reviewing downloads

In the dashboard, open the submission, find **Attachments**, then use **File** to view or download the upload.

Treat every unknown upload like any other unknown attachment: download only when you expect the file, scan it with your security tools, and be cautious before opening documents, archives, or executables.

## Limits

File limits are enforced before storage. Empty file inputs are ignored.

Current browser form support allows one non-empty attachment per submission, up to 4 MB.
