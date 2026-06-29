---
title: Examples
description: Copy working html.contact form examples for plain HTML, static sites, AI-built sites, website builders, and server-side HTTP posts.
---

Use these examples when you want a working form quickly. Each page starts with copyable code, then links back to the deeper docs when you need product rules, limits, or API details.

## Start here

| Example | Best for | What it shows |
| --- | --- | --- |
| [Simple HTML](/examples/simple-html-contact-form/) | Any website that can post a normal form | The smallest working contact form |
| [Production HTML](/examples/production-html-contact-form/) | Real contact pages | Reply-to, honeypot support, subject, and optional redirect |
| [Custom Fields](/examples/custom-fields/) | Intake, quote, and application forms | Arbitrary named fields and checkbox groups |
| [Attachments](/examples/attachments/) | Forms that need one uploaded file | `multipart/form-data` and one file input |
| [Astro contact form](/examples/astro-contact-form/) | Static Astro sites and Astro islands | A plain form in an Astro page |
| [Next.js contact form](/examples/nextjs-contact-form/) | App Router and static Next.js pages | A direct browser form post without a server action |
| [React contact form](/examples/react-contact-form/) | React apps and static React pages | A normal form rendered by React |
| [Vue contact form](/examples/vue-contact-form/) | Vue apps and static Vue pages | A normal form rendered by Vue |
| [Webflow contact form](/examples/webflow-contact-form/) | Designer-built sites | How to point a Webflow form at html.contact |
| [AI website builders](/examples/ai-website-builders/) | Lovable, Bolt, v0, Replit, Cursor, Claude, and Codex workflows | A prompt your builder can follow |
| [Server-Side POST](/examples/http-post/) | Server scripts, jobs, and custom backends | cURL, Node, Python, and PHP examples |

## Endpoint styles

The recommended style puts the public form key in the form action:

```html
<form action="https://html.contact/f/hc_pub_REPLACE" method="POST">
```

The compatibility style posts to `/submit` and sends the public key as `form_key`:

```html
<form action="https://html.contact/submit" method="POST">
  <input type="hidden" name="form_key" value="hc_pub_REPLACE">
</form>
```

Both styles use the same spam screening, domain allowlist, delivery, logging, and attachment flow.

Use `/f/hc_pub_REPLACE` first. Use `/submit` only when you need the public key in the form body.

## Useful docs

- [HTML Forms](/docs/html-forms/)
- [Domains & Spam](/docs/domains-and-spam/)
- [Attachments](/docs/attachments/)
- [API Overview](/docs/api/)
