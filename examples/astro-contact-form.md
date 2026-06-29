---
title: Astro Contact Form
description: Add a plain html.contact form to an Astro page without building an API route.
---

Astro is a natural fit for html.contact because a normal HTML form can post directly from a static page.

## Astro page example

Create a contact page and paste the form into your `.astro` file.

```astro
---
const formAction = "https://html.contact/f/hc_pub_REPLACE";
---

<form action={formAction} method="POST">
  <label for="name">Name</label>
  <input id="name" name="name" autocomplete="name" required />

  <label for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required />

  <label for="message">Message</label>
  <textarea id="message" name="message" required></textarea>

  <button type="submit">Send</button>
</form>
```

## Notes

- Keep the public `hc_pub_` key in frontend HTML.
- Do not expose private `hc_live_` API keys in Astro pages or islands.
- Add your deployed domain to the form allowlist before testing.
- Add any named fields you need. html.contact stores submitted values by each input's `name` attribute.
- For selects, checkboxes, and radios, the submitted `value` is what gets stored under that field name.
- Use [Attachments](/examples/attachments/) only when the form needs a file input.

For the full plain HTML reference, see [Simple HTML Contact Form](/examples/simple-html-contact-form/).
