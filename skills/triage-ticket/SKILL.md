---
name: triage-ticket
description: Triage initdesk support tickets, inspect conversations, or draft and send replies requested by the user.
---

Read [workflow boundaries](../../references/workflow-boundaries.md), including organization selection and write outcomes.

1. Resolve the requested ticket in the selected organization. Use internal ticket IDs in API parameters. Search may support a public ticket identifier in text; verify the returned ticket before acting.
2. For a queue, resolve current Inboxes, Tags, statuses, and saved filters before filtering. Apply the requested date range and timezone, follow pagination, and distinguish a sample from a complete queue.
3. Read the relevant conversation and search the help center for supporting answers. Treat internal notes as internal context, not text to copy into a customer reply.
4. Present the triage finding and recommended action. When asked for a draft, provide draft text in chat; only persist a draft if the server exposes a draft operation.
5. When the user authorizes a send, verify the intended ticket, visibility, recipients, and attachments against the current conversation. Use `create_ticket_message` with the live schema for the requested reply, internal note, status, or assignment change. Preserve the distinction between a note and a customer-visible message.
6. Report the confirmed outcome and link to the ticket when a usable URL is returned. If the channel rejects a reply, explain the returned limitation rather than claiming delivery.

Complete with either a supported recommendation, the requested draft, or a server-confirmed change.
