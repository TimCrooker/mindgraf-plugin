# MindGraf Plugin for Cowork

Knowledge graph superpowers for Claude. Full access to the MindGraf second brain via the brain CLI, plus workflow skills for fitness, communications, project management, journaling, research, and daily life.

## Setup

1. Import `mindgraf.plugin` into Cowork (drag into chat or use Customize sidebar)
2. When prompted, set `mindgraf_path` to your secondbrain-claude repo path (e.g. `/Users/you/secondbrain-claude`)
3. Ensure MindGraf Desktop is running on the host Mac
4. Connect required MCP servers in Cowork (see CONNECTORS.md)

## Skills (11)

| Skill | Purpose |
|-------|---------|
| **brain-cli** | Core entity operations: search, create, read, edit, link, tag, status, dashboard, graph |
| **hevy** | Workout tracking: view workouts, routines, exercise history, PRs, build programs |
| **calendar** | Google Calendar: view, create, update, delete events, find free time |
| **email** | Gmail: read inbox, search, read threads, send/reply with confirmation |
| **messaging** | iMessage: send/read texts, contact resolution, conversation history |
| **contacts** | Contact lookup: brain graph, iMessage MCP, macOS Contacts, entity creation |
| **reminders** | Apple Reminders: view, create, complete, batch create, list management |
| **pm** | Project management: goals, projects, work items, decisions, daily/weekly loops |
| **journal** | Daily journal: Six S-Tier Pillar tracking (training, cardio, piano, spanish, AI, writing) |
| **research** | Web research into knowledge graph with source attribution and dedup |
| **quick-capture** | Fast "throw it in the brain" capture: URLs, people, tools, thoughts, tasks |

## Agents (1)

| Agent | Purpose |
|-------|---------|
| **brain-protocol** | Knowledge graph context lookup — dispatched to search the brain before substantive responses |

## Hooks

- **SessionStart**: Reminds Claude that the brain is available and should be searched proactively on substantive queries.

## Architecture

```
User query
  |
  v
[SessionStart hook: "brain is available, use it"]
  |
  v
[Skill auto-triggers on keyword match]
  |
  v
[brain CLI via osascript] --> MindGraf Desktop API --> Knowledge Graph
  |
  v
[Native MCP tools] --> Gmail / Calendar / iMessage / Contacts
```

All brain CLI commands execute on the host Mac via the "Control your Mac" MCP server's `osascript` tool. The `mindgraf_path` userConfig variable is substituted into invocation patterns at plugin load time.

## Requirements

- MindGraf Desktop running on the host Mac
- Control your Mac MCP connected in Cowork
- Gmail, Google Calendar, iMessage MCPs connected for comms skills
