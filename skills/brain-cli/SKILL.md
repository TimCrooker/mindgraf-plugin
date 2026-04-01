---
name: brain-cli
description: >
  Core MindGraf brain CLI skill for Cowork. Use when the user asks to "search the brain",
  "create an entity", "read a project", "link entities", "check the dashboard",
  "search my knowledge graph", "what do I have on X", or any knowledge graph operation.
  Also triggers on: brain, mindgraf, entity, knowledge graph, graph search.
---

# Brain CLI — Cowork Integration

The brain CLI is the primary interface to MindGraf, a personal knowledge graph.
In Cowork, invoke it via the Mac control MCP since the CLI runs on the host machine.

## Invocation Pattern

Every brain command follows this template:

```
mcp__Control_your_Mac__osascript with script:
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

Abbreviate as `brain <command>` in planning. Always use `--json` when parsing output.

## Pre-Flight Check

Before first brain command in a session, verify the API is reachable:

```
brain list --count --json
```

If it errors with "API is unreachable", tell the user: "MindGraf Desktop isn't running — start it and I'll retry."

## Core Commands

### Search (always do this before creating)
```
brain search "<query>" [--type T] [--status S] [--tag T] [--limit N] [--json]
```
Sources: entity, gmail, imessage, gcalendar, hevy. Use `--source S` to filter.

### Read
```
brain read <ref> [--json] [--editable] [--no-context] [--full]
```
Refs accept: title, `type/Title`, `[[wiki link]]`, or UUID.

### Create
```
brain create <type> <title> [--flags] [--body TEXT] [--force] [--json]
```
Supported types: project, goal, decision, work_item, person, organization, tool, resource, note, journal.

Always search before creating. Use `--force` only to skip dupe check intentionally.

### Edit
Three modes (mutually exclusive):
- Surgical: `brain edit <ref> --old "X" --new "Y" [--replace-all] [--dry-run]`
- Append: `brain edit <ref> --append "text"`
- Parent: `brain edit <ref> --parentRef <ref>`

### Status / Complete / Archive
```
brain status <ref> <status>    # Validates per type
brain complete <ref>           # project/goal→completed, decision→revised, work_item→completed
brain archive <ref> [--yes]    # work_item→cancelled, others→archived
```

### Link / Unlink / Tag
```
brain link <src> <dst> [--type edge]   # edge types: related, depends_on, content_ref, parent_of, used_in, member_of, authored_by
brain unlink <src> <dst>
brain tag <ref> add|remove <tags...>
```

### List
```
brain list [type] [--status S] [--limit N] [--offset N] [--sort FIELD] [--json]
```
Without type: shows entity counts. Sort: title, priority, status, createdAt, updatedAt.

### Dashboard
```
brain dashboard [--json]       # Overdue work items + decisions due for revisit
```

### Graph & Integrity
```
brain graph neighbors <ref> [--depth N] [--json]
brain integrity audit [--json]
brain activity feed [--days N] [--json]
```

### Communications (cached search)
```
brain comms search <q> [--source S] [--contact C] [--limit N]
brain comms upcoming [--days N]
brain comms contacts [--source S] [--search Q]
brain comms thread <threadId> [--limit N]
```

## Entity Types & Valid Statuses

| Type | Statuses | Key Create Flags |
|------|----------|------------------|
| project | active, paused, completed, archived | --priority, --parentRef |
| goal | active, at_risk, completed, archived | --targetDate, --targetValue, --unit |
| decision | active, revisit_due, revised, archived | --confidence, --rationale, --revisitAt |
| work_item | pending, in_progress, completed, cancelled | --priority, --dueDate, --projectRef, --goalRef, --parentRef |
| person | active, archived | --firstName, --lastName, --email, --phone, --company, --role, --relationship |
| organization | active, archived | --industry, --size, --website, --location |
| tool | active, dropped | --category, --url, --pricing, --toolStatus |
| resource | to-read, completed, archived | --author, --url, --medium, --resourceStatus, --rating |
| note | active, archived | --folder, --noteType, --parentRef |
| journal | (auto-named) | --date, --rating, --custom-fields |

## Critical Rules

1. **Wikilinks**: `[[entity name]]` in body text auto-creates graph edges. ALWAYS use them for entity references in body content.
2. **Search before creating**: Always `brain search "title" --json` before `brain create`.
3. **Use --json** when parsing output programmatically.
4. **Use brain status** over brain edit for status changes.
5. **Metadata in flags, not body**: Structured data goes in create/edit flags. Body is for narrative.

See `references/entity-flags.md` for complete create flag reference.
