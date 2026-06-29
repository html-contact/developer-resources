---
title: Next.js Contact Form
description: Add a direct html.contact form post to a Next.js page without creating a backend route.
---

The simplest Next.js setup is still a normal HTML form. You do not need a server action unless your app has custom validation or business logic before submission.

## App Router component

```tsx
export function ContactForm() {
  return (
    <form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
      <label htmlFor="name">Name</label>
      <input id="name" name="name" autoComplete="name" required />

      <label htmlFor="email">Email</label>
      <input id="email" name="email" type="email" autoComplete="email" required />

      <label htmlFor="message">Message</label>
      <textarea id="message" name="message" required />

      <button type="submit">Send</button>
    </form>
  );
}
```

## Notes

- This form posts from the browser to html.contact.
- The public `hc_pub_` key is safe in client-rendered markup.
- Private `hc_live_` keys belong only in trusted server code.
- Add your production domain to the form allowlist.
- Add any named fields you need. html.contact stores submitted values by each input's `name` attribute.
- For selects, checkboxes, and radios, the submitted `value` is what gets stored under that field name.
- Use [Attachments](/examples/attachments/) only when the form needs a file input.

If you need a server-side script or API route to submit data, see [Server-Side POST](/examples/http-post/).
