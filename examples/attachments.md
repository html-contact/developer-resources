---
title: Attachments Example
description: Add one browser file input to a public html.contact form.
---

Use `multipart/form-data` only when the form includes a file input. Replace `hc_pub_REPLACE` with the public form key from your form.

```html
<form
  action="https://html.contact/f/hc_pub_REPLACE"
  method="POST"
  enctype="multipart/form-data"
>
  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required>

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required>

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <label for="attachment">Attachment</label>
  <input id="attachment" name="attachment" type="file">

  <button type="submit">Send</button>
</form>
```

## What changes with files

- The form needs `enctype="multipart/form-data"`.
- Current browser form support allows one non-empty attachment per submission, up to 4 MB.
- Empty file inputs are ignored.
- The file input still needs a `name` attribute. The uploaded filename is stored with the submitted fields under that name.
- Email notifications link to authenticated downloads instead of attaching raw files directly.
- In the dashboard, open the submission, find **Attachments**, then use **File** to view or download the upload.

Treat every unknown upload like any other unknown attachment: download only when you expect the file, scan it with your security tools, and be cautious before opening documents, archives, or executables.

For more detail, see [File Uploads](/docs/attachments/).
