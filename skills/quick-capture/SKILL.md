---
name: quick-capture
description: >
  Fast knowledge graph capture with minimal friction. Use when the user wants to quickly save
  something to their brain — a URL, a person, a thought, a tool, a note, contact info, or any
  piece of information. Triggers on: save this, capture this, remember this, add to brain,
  note this, quick note, jot down, bookmark this, save for later.
---

# Quick Capture Skill

Rapidly ingest information into the MindGraf knowledge graph with minimal back-and-forth.
This is the "just throw it in" path — faster than research or mine for the 80% case.

## Invocation
```
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

## Core Principle
Classify -> Dedupe -> Create -> Link. One confirmation, then done.

## Classification Rules

Parse the input and auto-detect entity type:

| Input Pattern | Entity Type | Key Flags |
|---------------|-------------|-----------|
| URL (article, blog, video) | resource | --url, --medium, --author |
| Person name + context | person | --firstName, --lastName, --email, --company, --relationship |
| Company/org name | organization | --industry, --website |
| Product/service/app name | tool | --category, --url, --pricing |
| Explicit decision/tradeoff | decision | --rationale, --chosenOption, --confidence |
| General thought/idea/concept | note | --noteType, --folder, tag `concept` |
| Task/action item | work_item | --priority, --dueDate, --projectRef |

## Workflow

### 1. Classify
Parse user input. Determine entity type from context. If ambiguous, make best guess and include in confirmation.

### 2. Dedupe (Always)
```
brain search "<title or key phrase>" --json
```
If strong match exists: offer to enrich instead of duplicate.

### 3. Prepare Create
Build the create command with all extractable metadata in flags (not body).
Put narrative/context in `--body` with [[wikilinks]] for any referenced entities.

### 4. Single Confirmation
Show one compact confirmation:
```
Create [type]: "[title]"
  [key metadata fields]
  [body preview if any]
  [links: -> entity1, -> entity2]
OK?
```

### 5. Execute
On confirmation:
1. `brain create <type> "<title>" --flags --body "..." --json`
2. `brain link "<new>" "<related>"` for any relevant connections
3. `brain tag "<new>" add <tags>` if tags are obvious

### 6. Confirm
Report: "Created [type] '[title]' + [N] links."

## Speed Rules
- Default to creating, not discussing
- Extract maximum metadata from context automatically
- One confirmation step, not multiple
- If the user says "save this" about something in conversation, capture the relevant context without asking what to capture
- For URLs: fetch and summarize automatically, create resource with synthesis

## Bulk Capture
If the user provides multiple items (list of tools, people, URLs), process each:
1. Show full batch plan
2. Single confirmation for the batch
3. Execute all creates + links
4. Report summary count
