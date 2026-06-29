---
title: Custom Fields
description: Add arbitrary named fields to html.contact forms and understand how they are stored.
---

html.contact stores named form fields as submitted. Add the fields your form actually needs.

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
  <label for="name">Name</label>
  <input id="name" name="name" required>

  <label for="company">Company</label>
  <input id="company" name="company">

  <label for="budget">Budget</label>
  <select id="budget" name="budget">
    <option value="under-5k">Under $5k</option>
    <option value="5k-15k">$5k - $15k</option>
    <option value="15k-plus">$15k+</option>
  </select>

  <label for="timeline">Timeline</label>
  <input id="timeline" name="timeline">

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <button type="submit">Send</button>
</form>
```

## Field rules

- Every field you want to receive needs a `name` attribute.
- The stored field key is the `name`. The stored field value is the submitted value, such as the text a visitor typed or the selected option's `value`.
- Unknown fields are preserved in submission data, API detail, and CSV exports. The dashboard table prioritizes common fields and may not show every custom field as a column.
- Duplicate field names are stored as multiple values.
- `subject` is stored as submitted data, can appear in the dashboard table, and can set the notification subject. The dashboard default subject is used only when no submitted `subject` field exists.
- `_subject` and `_intro` are stored as normal submitted fields and do not override notification subject or intro. The dashboard default intro is the intro used in notification emails.
- `_to`, `_cc`, `_bcc`, and `_from` are stored as ordinary submitted fields if used, but they never route email.

## Checkbox groups

Use duplicate names when visitors can choose multiple values.

```html
<label>
  <input type="checkbox" name="services" value="Design">
  Design
</label>

<label>
  <input type="checkbox" name="services" value="Development">
  Development
</label>
```

Both selected values are preserved with the submission.
