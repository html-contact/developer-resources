---
title: Overview
description: Learn how html.contact turns normal HTML forms into email notifications and submission logs.
---

html.contact is an instant form backend for static sites, AI-built sites, personal projects, and small business websites.

Create an endpoint, paste a normal HTML form, add your website hostname, and receive submissions by email with logs in your account.

## How it works

1. Create a form and copy its public `hc_pub_` endpoint.
2. Paste a normal HTML form into your website.
3. Send submissions to your verified inbox.
4. Review received posts, spam, and attachments in your account.

## Common links

- [Add a Contact Form](/docs/getting-started/)
- [HTML Forms](/docs/html-forms/)
- [Form Fields](/docs/form-fields/)
- [Troubleshooting](/docs/troubleshooting/)
- [Examples](/examples/)

## Private API

Use the [Private API](/docs/api/) only for trusted servers, automations, and approved agents. Private `hc_live_` keys should never appear in frontend code.

## MCP private preview

Invited coding-agent clients can follow the [MCP private-preview guide](/docs/mcp/). MCP uses scoped OAuth rather than `hc_live_` API keys, and live server metadata remains the source of truth for its tool contract.

## Product status

The current product focuses on simple form endpoints, email delivery, source checks, spam screening, logs, and authenticated attachment downloads.
