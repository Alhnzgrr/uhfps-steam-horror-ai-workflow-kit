# UHFPS Motion Controller Skill

## Purpose

Use this skill when adding camera motion, item sway, external motion events,
impact wobble, breathing, or player item motion feedback.

## Key UHFPS Types

- `MotionController`
- `MotionPreset`
- `MotionBlender`
- `MotionModule`
- `SimpleMotionModule`
- `SpringMotionModule`
- `ExternalMotions`
- `ExternalMotion`
- `PlayerItemBehaviour.ApplyEffect(string eventID)`

## Motion Concepts

UHFPS blends motion modules by player state. Motion states can target walking,
idle, crouch, jump, land, or custom player states.

For player items:

- assign `MotionPreset`
- enable `EnableMotionPreset`
- set `MotionPivot` or use default camera hands pivot
- add external motion states when item actions should kick camera/item motion

Use `ApplyEffect(eventID)` from a `PlayerItemBehaviour` to play external motion
configured on that item.

## Creating Motion Modules

Use:

- `SimpleMotionModule` for simple deterministic offsets
- `SpringMotionModule` for spring-like camera/item movement

Custom modules should expose readable tuning values and avoid allocations in
per-frame motion updates.

## Rules

- Do not add camera shake everywhere.
- Match motion intensity to horror tone and player vulnerability.
- Do not hide important gameplay state with excessive wobble.
- Do not break aiming, examining, or puzzle readability.
- Do not create parallel camera motion systems when UHFPS motion supports the use case.

## Setup Checklist

- MotionPreset assigned where needed
- MotionPivot selected
- default MotionController has required external motion module
- external motion EventID matches code call
- amplitude/frequency/duration tuned for PC horror
- pause, cutscene, and examine behavior reviewed

## Validation

- Motion appears only during intended state/action.
- Motion stops cleanly after item deselect or cutscene.
- No nausea-inducing repeated motion.
- External motion missing module produces a clear setup error.
