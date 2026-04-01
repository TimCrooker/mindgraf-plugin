---
name: messaging
description: >
  Send and read iMessages. Use when the user wants to text someone, check messages, read
  conversations, or send a message. Triggers on: text, message, iMessage, send, tell,
  reply, check messages, what did X say.
---

# Messaging Skill

Send and read iMessages via the iMessage MCP tools (send_imessage, read_imessages, get_unread_imessages, search_contacts).

## SAFETY — NEVER send a message without explicit user confirmation.
Before every send:
1. Resolve recipient via contacts skill
2. Show: "Send to [Full Name] ([phone]): [message]"
3. Wait for "yes", "send", "confirm"
4. Only then execute

If the user has already confirmed (provided names and said "yes"), do NOT ask again.

## Commands

### Send Message
Trigger: "text X", "message X", "tell X"

1. Parse recipient name and message content
2. Resolve via contact resolution flow (see below)
3. Show confirmation
4. On confirmation, use `send_imessage`

### Read Messages
Trigger: "texts", "messages", "check messages", "unread"

Use `get_unread_imessages` for unread, `read_imessages` for conversation history.

### Read Conversation
Trigger: "texts from X", "messages from X", "what did X say"

1. Resolve person's phone via contacts
2. Use `read_imessages` filtered to that contact

## Contact Resolution Flow
1. **Brain graph first**: `brain search "<name>" --type person --json` — check for entity with phone/email
2. **iMessage contacts**: Use `search_contacts` MCP tool
3. **macOS Contacts**: Fall back to osascript lookup (open -a Contacts first)
4. **Disambiguate**: Single match → use it. Multiple → show options, ask. None → ask for phone number.

Handle common nicknames: Gabby → Gabi, Dave → David, Jim → James, Tim → Timothy.

## Limitations
- macOS Messages AppleScript CANNOT create new group chats
- Can send to existing group chats if you know the chat ID
- For new groups, offer to send individually to each person

## Brain Integration
- Use `brain search` to find person entities with relationship context
- Match contact info from brain entities before falling back to Contacts app

## Output Style
- Show sender, timestamp, message content
- Group by conversation
- Most recent first
