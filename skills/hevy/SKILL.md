---
name: hevy
description: >
  Hevy workout tracking — view workouts, manage routines, browse exercises, analyze training
  history, and build new workout programs. Use when the user asks about workouts, training,
  lifting, gym, exercises, sets, reps, PRs, routines, workout history, progressive overload,
  or wants to build/plan a workout. Triggers on: workout, training, lift, gym, exercise,
  sets, reps, PR, routine, program, hevy, build workout, plan workout, workout history,
  last workout, gains, progressive overload, split, push pull legs, upper lower.
---

# Hevy Skill

Workout tracking and program building via the Hevy API v1.

## Invocation
All brain/hevy operations via Mac control MCP:
```
do shell script "export MINDGRAF_PATH=${user_config.mindgraf_path} && ${user_config.mindgraf_path}/apps/cli/bin/brain <command> 2>&1"
```

For Hevy API operations, use the `@sb/skills` TypeScript package functions via the brain CLI agent, or call the Hevy API directly.

**API key**: stored at `~/.config/mindgraf/hevy-api-key` (Hevy Pro required).
**Cache**: `~/.cache/mindgraf/hevy/` — 24h TTL, auto-refreshes.
**Weight units**: API stores kg. Convert: `weight_kg * 2.20462 = lbs`. Default to lbs for display.

## Commands

### Recent Workouts
Trigger: "workouts", "last workout", "what did I do", "training history"

Fetch recent workouts via the Hevy API. Format as numbered list with date, duration, top exercises.

Display rules:
- Convert kg to lbs, round to nearest whole number
- Format sets: `225x8` (weight x reps)
- Combine identical sets: `225x8 x3`
- Duration = end_time - start_time, format as `Xh Ym`
- Sort most recent first

### Exercise History / PRs
Trigger: "bench press history", "PR on squat", "progress on X", "how much do I X"

1. Find exercise template ID by searching exercises
2. Fetch history for that exercise
3. Show recent sessions, volume, 1RM estimate, trend

**1RM estimation** (Epley): `1RM = weight * (1 + reps/30)`
**Volume** per session: `sum(weight * reps)` for working sets.

### List/View Routines
Trigger: "routines", "my routines", "show routine X"

List all routines with exercise/set counts. For details, show full exercise list with target sets/reps/weight.

### Build Workout / Routine
Trigger: "build me a workout", "create a routine", "make a push day", "design a program"

**MUST confirm before creating.**

Workflow:
1. Gather: goal (strength/hypertrophy/endurance), split type, target muscles, equipment, time, experience level
2. Check exercise history for weight selection
3. Design routine following training principles
4. Show proposed routine — confirm
5. On confirmation, create via API

Critical: API expects `weight_kg`. Convert lbs to kg: `lbs / 2.20462`.

### Build Full Program
Trigger: "build me a program", "create a 4-day split", "design a PPL"

Same as Build Workout but for multiple routines. Gather days/week, split preference, goal, constraints. Design each routine, show full program, confirm, create all.

### Update Routine
Trigger: "update routine", "change routine", "swap exercise"

**Show current vs proposed, confirm before updating.**

PUT replaces entire routine. Always send complete routine object. Do NOT include `folder_id` in PUT, do NOT include `index` in sets.

### Training Stats
Trigger: "stats", "training stats", "workout count"

Compute: total workouts, this week/month, avg duration, most trained muscles, frequency.

## Data Model (Quick Reference)

**Set types**: normal, warmup, failure, dropset
**Exercise types**: weight_reps, reps_only, duration, distance_duration, weight_distance, weight_duration
**Muscle groups**: chest, back, shoulders, biceps, triceps, forearms, abdominals, quads, hamstrings, glutes, calves, cardio

See `references/hevy-data-model.md` for full JSON schemas, routine create format, and training principles.

## Journal Integration
Workout data feeds the Training pillar of the daily journal:
- `exercise_minutes` = workout duration
- Reference workout in Training section

## Safety Rules
1. Read operations (list, view, history) — safe, run automatically
2. Create routine/workout — MUST show details and get confirmation
3. Update routine — MUST show diff and get confirmation
4. No delete via API — must be done in Hevy app
5. Always double-check kg/lbs conversion before creating

## Output Style
- Convert kg to lbs (round nearest 5 for display, exact for API)
- Compact set notation: `225x8` not `225 lbs x 8 reps`
- Group identical sets: `225x8 x3`
- Duration: `Xh Ym`
- Concise, no filler
