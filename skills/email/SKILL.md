---
name: email
description: >
  Read and send emails via Gmail. Use when the user asks about email, inbox, wants to send
  an email, check recent emails, or reply to messages. Triggers on: email, inbox, mail,
  send email, reply, unread, gmail, check email.
---

# Email Skill

Read and send emails via Gmail MCP tools (gmail_search_messages, gmail_read_message, gmail_read_thread, gmail_create_draft, gmail_list_labels).

Account: timothycrooker@gmail.com

## SAFETY — NEVER send an email without explicit user confirmation.
Before every send:
1. Show: "Send to [recipient] — Subject: [subject]\n\n[body preview]"
2. Wait for explicit "yes", "send", or "confirm"
3. Only then execute

## Commands

### Read Inbox
Trigger: "email", "inbox", "check email", "unread"

Use `gmail_search_messages` with appropriate query. Format as clean list with unread marked `[NEW]`.

For unread only: query `in:inbox is:unread`.

### Search Emails
Trigger: "email from X", "emails about Y"

Gmail query syntax:
- `from:alice@example.com` — from sender
- `to:bob@example.com` — to recipient
- `subject:meeting` — subject contains
- `after:2025/01/01` — after date
- `before:2025/06/01` — before date
- `is:unread` — unread only
- `has:attachment` — with attachments

Combine freely: `from:alice subject:project after:2025/01/01 has:attachment`

### Read Email
Trigger: "read email about X", "open email from X"

Use `gmail_read_message` with the message ID from search results.

### Read Thread
Use `gmail_read_thread` for full conversation context.

### Send / Reply
Trigger: "email X about Y", "send email to", "reply to"

1. Resolve recipient via contacts skill (brain search → macOS Contacts)
2. Compose based on user intent
3. Show full confirmation (REQUIRED)
4. On confirmation, use `gmail_create_draft` then confirm send

### Brain Integration
- Use `brain search "<person>" --type person` to resolve recipients and add context
- Link conversations to projects/entities when relevant
- Use `brain comms search` for cached email search across the graph

## Output Style
- Show sender, subject, date, snippet
- Mark unread with `[NEW]`
- Keep thread summaries concise
- Include message count for search results
