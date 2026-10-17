---
title: Overview
description: Connect HTML forms to email, Slack, webhooks, and AI agents, with Google Sheets available to enabled pilot accounts.
---

html.contact is an instant form backend for static sites, AI-built sites, personal projects, and small business websites.

Create an endpoint, paste a normal HTML form, add your website hostname, and receive submissions by email with logs in your account.

## How it works

1. Create a form and copy its public `hc_pub_` endpoint.
2. Paste a normal HTML form into your website.
3. Send submissions to your verified inbox.
4. Review received posts, spam, and attachments in your account.

## Common links

- [Add a Contact Form](getting-started.md)
- [HTML Forms](html-forms.md)
- [Form Fields](form-fields.md)
- [Troubleshooting](troubleshooting.md)
- [Examples](../examples/index.md)

## Agency API

Agency accounts use the [Agency API](api.md) for trusted server-side client, form, submission, export, and attachment automation. Account-owned `hc_live_` Agency API keys should never appear in frontend code or prompts.

## Connect your tools

- [Slack](slack.md): send new inquiries to a team channel, with selected fields or a link to the full response.
- [Signed webhooks](webhooks.md): deliver new responses to your app as JSON, with delivery history and retries.
- [Google Sheets](google-sheets.md): append new responses to a dedicated worksheet with automatic columns. Currently limited to enabled pilot accounts.

Slack and webhooks are included on every plan. Google Sheets has no paid-plan requirement; its availability is still limited.

## Connect with MCP

Verified, fully onboarded accounts can connect supported AI and coding-agent clients with [scoped MCP access](mcp.md). Create and troubleshoot forms, get installation instructions, and approve configuration changes from your agent. No invitation or `hc_live_` API key is required. The ChatGPT app is being prepared for directory review; its listing is not published yet.

## Product status

Keep your own form design. html.contact handles response collection, source checks, spam screening, email notifications, connected destinations, and submission history.
