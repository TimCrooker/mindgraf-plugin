---
name: contacts
description: >
  Look up contact information from macOS Contacts and the MindGraf knowledge graph. Use when
  the user asks about a person's phone number, email, or contact info, or when resolving
  a recipient for messaging/email. Triggers on: contact, phone number, email for, look up,
  find contact, who is.
---

# Contacts Skill

Look up people from macOS Contacts (514 contacts synced) and MindGraf person entities.

## Resolution Flow (Enhanced)

When resolving a contact for any purpose:

1. **Check brain graph**: `brain search "<name>" --type person --json`
   - Look for person entities with phone/email in metadata
   - If found, use `brain read <ref> --json` to extract contact info

2. **iMessage MCP**: Use `search_contacts` for quick lookup

3. **macOS Contacts** (via osascript):
   ```
   do shell script "open -a Contacts && sleep 2 && osascript -e 'tell application \"Contacts\"
     set results to every person whose name contains \"SEARCH_TERM\"
     ...
   end tell'"
   ```
   MUST `open -a Contacts && sleep 2` before any osascript query.

4. **Disambiguate**:
   - Single match → return
   - Multiple matches + one has brain entity → prioritize that one, confirm
   - Multiple matches, no graph context → show all, ask
   - No match → say so, ask for phone/email

## Create Brain Entity
When a contact is important enough to track:
```bash
brain create person "Jane Smith" \
  --firstName Jane --lastName Smith \
  --email jane@example.com --phone "+1-555-0123" \
  --company "Acme Corp" --role "Engineering Manager" \
  --relationship colleague
```

Always populate all available metadata flags. Use `--body` for relationship context.

## Important
- Contact data is READ-ONLY (never modify macOS Contacts)
- For messaging, always use phone number in send commands
- Wrap optional fields in try/end try in AppleScript
- Batch export (50 at a time) for bulk operations — iterating all 514 at once will timeout
