---
name: reminders
description: >
  Apple Reminders management with strong disambiguation and confirmation. Use when the user
  wants to set reminders, check due items, manage lists, complete tasks, or clean up reminders.
  Triggers on: remind me, reminder, reminders, don't forget, due items, groceries, todo,
  check off, complete task.
---

# Reminders Skill

Safe CRUD for Apple Reminders via osascript on the host Mac.

## Invocation
All operations via Mac control MCP. Must pre-open app:
```
do shell script "open -a Reminders && sleep 2"
```

## Priority Mapping
| Value | Display | Input keywords |
|-------|---------|----------------|
| 0 | (none) | default |
| 1 | !!! | urgent, critical, high |
| 5 | !! | medium |
| 9 | ! | low |

## List Selection Guide
| Context | List |
|---------|------|
| groceries, food, buy, eggs, milk | Groceries |
| work, deploy, meeting, project | Work |
| personal, family, birthday, gift | Personal |
| errands, pickup, drop off, return | Errands |
| default | Reminders |

Check available lists before assigning. If suggested list doesn't exist, use "Reminders" and offer to create it.

## Commands

### View Reminders
Trigger: "reminders", "my reminders", "show reminders", "what's due"

Group into: Overdue → Due Today → Upcoming → No Due Date. Show list name, priority indicators, due dates.

### Create Reminder
Trigger: "remind me", "add reminder", "remember to", "don't forget"

**MUST confirm before creating.**

Parse: title, due date, priority, list, notes. Show confirmation, then create.

### Batch Create
Trigger: "add to Groceries: eggs, milk, bread"

Parse items and list, confirm, create each.

### Complete Reminder
Trigger: "done X", "complete X", "finished X", "check off X"

If single match → complete. If multiple → show candidates, ask.

### Update / Delete
**MUST show details, confirm before modifying.**

For delete, hint that "complete" might be intended.

### List Management
Show lists, create lists, delete lists (with warning about removing all reminders).

## Safety Rules
1. View/search — safe
2. Create/update/delete/batch — require confirmation
3. Complete/delete — only on exactly one unambiguous match
4. Distinguish "delete" vs "complete"

## Output Style
- Concise, show list name and priority indicators
- Friendly due dates and overdue age
- Group: overdue → dated → no-date
