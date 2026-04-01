---
name: journal
description: >
  Daily journal entries with Six S-Tier Pillar tracking in MindGraf. Use when the user says
  "journal", "daily note", "log training", "log piano", "log spanish", "pillar tracking",
  "how was my day", "rate my day", or wants to create/update a journal entry.
---

# Journal Skill

Create and manage daily journal entries in MindGraf with Six S-Tier Pillar tracking.

## Invocation
```
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

## Daily Note Structure
- Naming: `YYYY-MM-DD DayName` (e.g., `2026-03-31 Tuesday`)
- Type: journal
- Fields: rating (1-10), exercise_minutes, vo2_minutes, piano_minutes, spanish_minutes, writing_minutes, ai_hours

## Six S-Tier Pillars

| Pillar | Field | Input Keywords |
|--------|-------|----------------|
| Training | exercise_minutes | training, exercise, workout, gym, lift |
| Cardio/VO2 | vo2_minutes | cardio, vo2, run, bike, swim |
| Piano | piano_minutes | piano, music, keys |
| Spanish | spanish_minutes | spanish, espanol, language |
| AI Systems | ai_hours | ai, coding, deep work |
| Writing | writing_minutes | writing, write, blog |

## Commands

### Create/Open Today's Journal
1. `brain search "YYYY-MM-DD" --type journal --json`
2. If not found: `brain create journal`
3. If found: `brain read journal/YYYY-MM-DD`

### Quick Log Pillar
Trigger: "log training 45", "log piano 30"

1. Determine today's journal (create if needed)
2. Parse pillar name and value from input
3. Update via `brain edit journal/YYYY-MM-DD` with appropriate field

Confirm: "Logged: [pillar] [value] [unit]"

### Add Journal Content
`brain edit journal/YYYY-MM-DD --append "<content>"`

Always use [[wikilinks]] for entity references in journal body.

### Entity Extraction
When creating/updating journals:
- Extract people -> `brain search`, create [[wikilinks]]
- Extract projects/tools -> link to existing entities

## Important
- One journal per day
- Never overwrite — always append
- Metric fields are numbers (no units)
- Six Pillars structure must be preserved
