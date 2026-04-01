# Hevy Data Model Reference

## Workout Object
```json
{
  "id": "uuid",
  "title": "Push Day",
  "description": "...",
  "start_time": "2026-02-05T14:00:00Z",
  "end_time": "2026-02-05T15:30:00Z",
  "exercises": [{
    "index": 0,
    "title": "Bench Press (Barbell)",
    "exercise_template_id": "uuid",
    "superset_id": null,
    "notes": "",
    "sets": [{
      "index": 0, "type": "normal",
      "weight_kg": 100, "reps": 8,
      "distance_meters": null, "duration_seconds": null, "rpe": null
    }]
  }]
}
```

## Routine Object
```json
{
  "id": "uuid",
  "title": "Push Day A",
  "folder_id": null,
  "exercises": [{
    "exercise_template_id": "uuid",
    "superset_id": null,
    "rest_seconds": 120,
    "notes": "",
    "sets": [{"type": "normal", "weight_kg": 100, "reps": 8}]
  }]
}
```

## Exercise Template Object
```json
{
  "id": "uuid",
  "title": "Bench Press (Barbell)",
  "type": "weight_reps",
  "primary_muscle_group": "chest",
  "secondary_muscle_groups": ["triceps", "shoulders"],
  "equipment": "barbell",
  "is_custom": false
}
```

## Set Types
- `normal` — working set
- `warmup` — warm-up set
- `failure` — set to failure
- `dropset` — drop set

API inconsistency: Workout/routine responses use `type`. Exercise history uses `set_type`. Handle both.

## Exercise Types
- `weight_reps` — standard weighted (bench, squat)
- `reps_only` — bodyweight (pull-ups)
- `duration` — timed (plank)
- `distance_duration` — cardio (running)
- `weight_distance` — weighted carry
- `weight_duration` — weighted hold

## Equipment Categories
barbell, dumbbell, machine, none (bodyweight), cable, band, kettlebell, other

## Muscle Groups
chest, back, shoulders, biceps, triceps, forearms, abdominals, quads, hamstrings, glutes, calves, cardio, other

## Routine JSON Body (CREATE via POST)
```json
{
  "routine": {
    "title": "Push Day A",
    "folder_id": null,
    "notes": "...",
    "exercises": [{
      "exercise_template_id": "<ID>",
      "superset_id": null,
      "rest_seconds": 120,
      "notes": "",
      "sets": [
        {"type": "warmup", "weight_kg": 61.24, "reps": 10},
        {"type": "normal", "weight_kg": 102.06, "reps": 8}
      ]
    }]
  }
}
```

## Routine Web URLs
Use `https://hevy.com/routine/{id}` where `{id}` is the routine UUID.

## Common Program Splits

### Push/Pull/Legs (6-day)
- Push A: chest-focused + shoulders + triceps
- Pull A: back-focused + biceps
- Legs A: quad-focused + calves
- Push B: shoulder-focused + chest + triceps
- Pull B: back width + biceps
- Legs B: hamstring/glute-focused + calves

### Upper/Lower (4-day)
- Upper A: horizontal push/pull focus
- Lower A: quad focus
- Upper B: vertical push/pull focus
- Lower B: hip hinge focus

### Full Body (3-day)
- Day A: squat pattern + horizontal push + horizontal pull
- Day B: hinge pattern + vertical push + vertical pull
- Day C: lunge pattern + push variation + pull variation

## Progressive Overload Guidelines
- Strength: add 5 lbs (barbell) or 2.5 lbs (dumbbell) when target reps achieved
- Hypertrophy: add reps within 8-12 range first, then add weight
- Track volume (sets × reps × weight) week over week
