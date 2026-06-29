---
title: Vue Contact Form
description: Add a direct html.contact form post to a Vue component without creating a backend route.
---

Vue can use a normal browser form post. No client SDK or API route is required for a basic contact form.

```vue
<template>
  <form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
    <label for="name">Name</label>
    <input id="name" name="name" autocomplete="name" required>

    <label for="email">Email</label>
    <input id="email" name="email" type="email" autocomplete="email" required>

    <label for="message">Message</label>
    <textarea id="message" name="message" required></textarea>

    <button type="submit">Send</button>
  </form>
</template>
```

## Notes

- Keep the public `hc_pub_` key in frontend markup.
- Never expose a private `hc_live_` key in Vue code.
- Do not submit this browser form with client-side `fetch` unless your app has a specific reason.
- Add the exact deployed hostname to your form's allowed domains before testing.
- Add any named fields you need. html.contact stores submitted values by each input's `name` attribute.
- For selects, checkboxes, and radios, the submitted `value` is what gets stored under that field name.
- Use [Attachments](/examples/attachments/) only when the form needs a file input.
