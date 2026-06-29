---
title: React Contact Form
description: Add a direct html.contact form post to a React component without creating a backend route.
---

React does not need special client JavaScript for a basic html.contact form. Render a normal form and let the browser submit it.

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

- Keep the public `hc_pub_` key in frontend markup.
- Never expose a private `hc_live_` key in React code.
- Do not submit this browser form with client-side `fetch` unless your app has a specific reason.
- Add the exact deployed hostname to your form's allowed domains before testing.
- Add any named fields you need. html.contact stores submitted values by each input's `name` attribute.
- For selects, checkboxes, and radios, the submitted `value` is what gets stored under that field name.
- Use [Production HTML](/examples/production-html-contact-form/) for reply-to, honeypot, subject, and redirect options.
