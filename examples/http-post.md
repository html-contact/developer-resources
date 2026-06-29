---
title: Server-Side POST
description: Send html.contact submissions from cURL, Node, Python, PHP, or any language that can make an HTTP POST.
---

Any language that can send an HTTP POST can submit to html.contact. These examples use public form endpoints with `hc_pub_` keys, not private API keys.

The public form endpoints accept form data, not JSON request bodies.

These examples are for servers, terminal tests, jobs, and custom backends. Do not copy the Node `fetch()` pattern into browser JavaScript. For websites, use a normal `<form>`.

Use the public form endpoint:

```txt
https://html.contact/f/hc_pub_REPLACE
```

Or use the compatibility endpoint with `form_key`:

```txt
https://html.contact/submit
```

## cURL

When testing with a normal website form configuration, include source headers that match an allowed domain:

```bash
curl "https://html.contact/f/hc_pub_REPLACE?format=json" \
  -H "Accept: application/json" \
  -H "Origin: https://example.com" \
  -H "Referer: https://example.com/contact" \
  --data-urlencode "name=Mara Quill" \
  --data-urlencode "email=mara@example.com" \
  --data-urlencode "subject=Quote request" \
  --data-urlencode "message=Can you send pricing?"
```

## Node

Server-side Node example:

```js
const body = new URLSearchParams({
  name: "Mara Quill",
  email: "mara@example.com",
  subject: "Quote request",
  message: "Can you send pricing?",
});

const response = await fetch("https://html.contact/f/hc_pub_REPLACE?format=json", {
  method: "POST",
  headers: {
    Accept: "application/json",
    "Content-Type": "application/x-www-form-urlencoded",
    Origin: "https://example.com",
    Referer: "https://example.com/contact",
  },
  body,
});

console.log(await response.json());
```

## Python

Server-side Python example:

```python
import requests

response = requests.post(
    "https://html.contact/f/hc_pub_REPLACE?format=json",
    headers={
        "Accept": "application/json",
        "Origin": "https://example.com",
        "Referer": "https://example.com/contact",
    },
    data={
        "name": "Mara Quill",
        "email": "mara@example.com",
        "subject": "Quote request",
        "message": "Can you send pricing?",
    },
)

print(response.json())
```

## PHP

Server-side PHP example:

```php
<?php
$response = file_get_contents(
    "https://html.contact/f/hc_pub_REPLACE?format=json",
    false,
    stream_context_create([
        "http" => [
            "method" => "POST",
            "header" => "Accept: application/json\r\nContent-Type: application/x-www-form-urlencoded\r\nOrigin: https://example.com\r\nReferer: https://example.com/contact\r\n",
            "content" => http_build_query([
                "name" => "Mara Quill",
                "email" => "mara@example.com",
                "subject" => "Quote request",
                "message" => "Can you send pricing?",
            ]),
        ],
    ])
);

echo $response;
```

## Language support

There is no language lock-in. html.contact works from any runtime that can send `application/x-www-form-urlencoded` or `multipart/form-data`.

That includes Node, Python, PHP, Ruby, Go, Java, C#, Swift, Kotlin, Bash, and most serverless platforms.

## Source checks

Normal browser forms should be tested from an allowlisted website domain.

Server-side posts without `Origin` or `Referer` headers require direct submissions to be enabled for that form. If direct submissions are off, expect `origin_missing`.

Only server code can set the example source headers shown above. Browser JavaScript cannot set `Origin` or `Referer` manually.
