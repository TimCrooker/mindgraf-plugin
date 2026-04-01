---
name: calendar
description: >
  Full Google Calendar management. Use when the user asks about their schedule, events,
  calendar, meetings, free time, or wants to create/update/delete events. Triggers on:
  calendar, schedule, event, meeting, free, busy, block, reschedule, cancel event, move meeting,
  what's on today, this week, my day.
---

# Calendar Skill

Full CRUD calendar management via Google Calendar MCP tools (gcal_list_events, gcal_create_event, gcal_update_event, gcal_delete_event, gcal_find_my_free_time, gcal_list_calendars).

## Calendar Selection Guide

Auto-select calendar from context:

| Context | Calendar |
|---------|----------|
| personal, dinner, date, appointment, errands, dentist | Personal (primary) |
| work, meeting, standup, 1:1, sync, call, deadline | Work schedule |
| workout, gym, run, tennis, lift, training, cardio | Workouts |
| focus, deep work, writing, study, project time | Knowledge |
| default / ambiguous | Personal |

For day views, show ALL calendars. For week+ views, filter to relevant ones (exclude Holidays unless asked).

## Commands

### View Today / Date Range
Trigger: "calendar", "schedule", "my day", "what's on today", "this week"

Use `gcal_list_events` with appropriate date range. Format:
- Separate all-day vs timed events
- Use 24h time
- Show calendar name and location
- Sort chronologically
- Include summary count for multi-day views

### Create Event
Trigger: "schedule", "add event", "block time", "create event"

**MUST confirm before creating.**

1. Parse: summary, date, start, end/duration (default 60m), calendar, location/description
2. Auto-select calendar from context
3. Show confirmation with all details
4. On confirmation, use `gcal_create_event`

### Update Event
Trigger: "move", "reschedule", "change event", "push back"

**MUST show current vs proposed, confirm before updating.**

Use `gcal_update_event`. Moving to different calendar = delete + create.

### Delete Event
Trigger: "cancel event", "delete event", "remove event"

**MUST show details, confirm before deleting.**

Use `gcal_delete_event`.

### Free Time
Trigger: "free today", "when am I free", "available time"

Use `gcal_find_my_free_time`. Show available blocks within waking hours (7am-10pm).

## Brain Integration
Link existing person and project entities via `brain search` in event context when relevant. Do not create new entities.

## Safety Rules
1. Read/list — safe, run automatically
2. Create — MUST confirm
3. Update — MUST show diff, confirm
4. Delete — MUST show details, confirm
5. Never modify Holidays calendar
6. Never bulk-delete
