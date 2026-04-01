# Entity Create Flags — Full Reference

## Person
```bash
brain create person "Jane Smith" \
  --firstName Jane --lastName Smith \
  --email jane@example.com --phone "+1-555-0123" \
  --company "Acme Corp" --role "Engineering Manager" \
  --relationship colleague
```

## Organization
```bash
brain create organization "Acme Corp" \
  --industry "Technology" --size "500-1000" \
  --website "https://acme.com" --location "San Francisco, CA" \
  --relationship partner
```

## Project
```bash
brain create project "Auth Overhaul" \
  --priority 1 --parentRef "Parent Project" \
  --body "Migrate to JWT. [[Security Team]] leading."
```
Priority: 0=P0/critical, 1=P1/high, 2=P2/default, 3=P3/nice-to-have.

## Work Item
```bash
brain create work_item "Write migration script" \
  --projectRef "Auth Overhaul" --goalRef "Ship Auth V2" \
  --priority 2 --dueDate 2026-03-01 --parentRef "Parent Task"
```

## Goal
```bash
brain create goal "Reach 1,000 paying customers" \
  --targetDate 2026-06-30 --targetValue 1000 --currentValue 50 --unit "customers"
```

## Decision
```bash
brain create decision "Use JWT over sessions" \
  --confidence 0.8 --rationale "Better for mobile" \
  --context "Evaluating auth strategies" \
  --chosenOption "JWT with refresh tokens" \
  --revisitAt 2026-04-01
```

## Resource
```bash
brain create resource "The Pragmatic Programmer" \
  --author "David Thomas" --url "https://..." \
  --medium book --resourceStatus to-read --rating 5
```
Medium options: book, article, video, podcast, paper, course, tool, website.

## Note
```bash
brain create note "Meeting Notes" \
  --folder "Meetings" --noteType "meeting" \
  --parentRef "Auth Overhaul" \
  --body "Discussed [[Auth Overhaul]] with [[Jane Smith]]."
```

## Journal
```bash
brain create journal  # auto-names as "YYYY-MM-DD DayName"
brain create journal --date 2026-03-15
brain create journal --rating 8 --custom-fields '{"exercise_minutes":45,"piano_minutes":30}'
```

## Tool
```bash
brain create tool "PostHog" \
  --category "Analytics" --url "https://posthog.com" \
  --pricing "Free tier + usage-based" --toolStatus active
```

## Common Flags (All Types)
- `--body TEXT` — Set body content (supports [[wikilinks]])
- `--body-file PATH` — Read body from file
- `--force` — Skip duplicate check
- `--json` — Return JSON output
