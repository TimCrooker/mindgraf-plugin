---
name: pm
description: >
  Outcome-first project management for MindGraf. Use when the user asks to "plan a project",
  "what should I work on", "weekly review", "daily planning", "create a task", "track a decision",
  "project status", "close a project", or any project/goal/task management request.
---

# Project Management Skill

Process: $ARGUMENTS

Turn intent into execution using the MindGraf PM model: Goal -> Project -> Work Item -> Decision.

## Invocation
All commands via Mac control MCP:
```
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

## First Principles
1. Outcomes before tasks: create/confirm the goal first, then project, then work.
2. Work must be executable: every in-progress item is concrete and finishable.
3. Priorities are explicit: P0-P3, no "everything is urgent."
4. Decisions are first-class: record rationale and revisit dates.
5. Graph coherence: link goals/projects/work items/decisions.

## Workflows

### Stand Up A New Initiative
Trigger: "start project X", "plan Y", "ship Z"
1. `brain create goal "<outcome>" --targetDate YYYY-MM-DD`
2. `brain create project "<name>" --priority P1 --body "<scope>"`
3. `brain link "<project>" "<goal>"`
4. Create 3-7 work items: `brain create work_item "<task>" --projectRef "<project>" --priority P1 --dueDate YYYY-MM-DD`
5. Capture tradeoffs: `brain create decision "<choice>" --confidence 0.7 --rationale "<why>" --revisitAt YYYY-MM-DD`

### Daily Operator Loop
Trigger: "what should I work on", "daily planning", "today plan"
1. `brain dashboard --json`
2. `brain list work_item --status in_progress --json`
3. `brain list work_item --status pending --json`
4. Select top 1-3 priorities
5. `brain status "<item>" in_progress`
6. `brain complete "<item>"` for done work
Keep WIP <= 3.

### Weekly PM Review
Trigger: "weekly review", "project review", "status check"
1. `brain dashboard --json`
2. `brain activity feed --days 7 --json`
3. `brain list project --json`
4. `brain list work_item --status pending,in_progress --json`
5. `brain list goal --json`
6. `brain integrity audit --json`
7. Apply: pause low-leverage projects, promote at-risk goals, prune stale tasks.

### Decision Hygiene
Trigger: choosing options, tradeoffs, architecture, vendors
1. `brain search "<topic>" --type decision --json`
2. `brain create decision "<title>" --confidence 0.8 --rationale "<why>" --chosenOption "<option>" --revisitAt YYYY-MM-DD`
3. `brain link "<decision>" "<project>"`

### Recovery (Stuck/Overloaded)
Trigger: "too many tasks", "stuck", "overwhelmed"
1. Surface stuck work, reduce WIP, split vague items, cancel dead work, re-anchor on one goal.

### Project Closeout
Trigger: "wrap project", "ship complete", "close project"
1. `brain read "<project>" --json`
2. Complete/cancel remaining work items
3. `brain complete "<project>"`
4. `brain edit "<project>" --append "<retro notes>"`

## Priority Mapping
- urgent/critical/blocker -> P0 (0)
- high/important -> P1 (1)
- default -> P2 (2)
- someday/nice-to-have -> P3 (3)

## Output Style
- Lead with actions taken
- Group entity refs by project
- Highlight blockers and overdue risk first
- Concise and execution-focused
