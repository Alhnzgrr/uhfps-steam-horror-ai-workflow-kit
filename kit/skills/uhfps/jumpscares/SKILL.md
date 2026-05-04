# UHFPS Jumpscares Skill

## Purpose

Use this skill when adding jumpscares, fear overlays, camera wobble, forced look,
or event-driven scare beats.

## Key UHFPS Types

- `JumpscareTrigger`
- `JumpscareManager`
- `JumpscareDirect`
- `WobbleMotion`
- `FearTentancles`
- `GameManager`
- `PlayerPresenceManager`

## JumpscareTrigger Configuration

Important enums:

- `JumpscareType`: `Direct`, `Indirect`, `Audio`
- `DirectType`: `Image`, `Model`
- `TriggerType`: `Event`, `TriggerEnter`, `TriggerExit`

Important fields:

- `JumpscareImage`
- `JumpscareModelID`
- `JumpscareSound`
- `Animator`
- `AnimatorStateName`
- `AnimatorTrigger`
- `InfluenceFear`
- `TentaclesIntensity`
- `TentaclesSpeed`
- `VignetteStrength`
- `LookAtJumpscare`
- `LookAtTarget`
- `LookAtDuration`
- `LockPlayer`
- `EndJumpscareWithEvent`
- `InfluenceWobble`
- `DirectDuration`
- `FearDuration`

`JumpscareTrigger` implements `ISaveable` and saves whether it already started.

## Event Flow

Use `TriggerJumpscare()` for event-driven scares.

For indirect animation scares:

- set `JumpscareType = Indirect`
- assign `Animator`
- set `AnimatorTrigger`
- set `AnimatorStateName`
- use `EndJumpscareWithEvent` only when an animation event or script will call
  `TriggerJumpscareEnded()`

## Design Rules

- Do not use jumpscares as filler.
- Build anticipation through sound, visibility, objective pressure, or spatial setup.
- Avoid repeated loud triggers that train the player to ignore the game.
- If `LockPlayer` is enabled, always verify the player is released.
- Use `InfluenceFear` and wobble as tone support, not as visual noise.

## Setup Checklist

- trigger collider set if using trigger enter/exit
- event caller wired if using event trigger
- direct image or model ID assigned
- sound assigned and mixed correctly
- look target assigned if forcing look
- fear overlay settings reviewed
- player lock/unlock behavior validated
- save/load tested so one-shot scares do not replay

## Validation

- Scare triggers once unless intentionally repeatable.
- Player is not left frozen or look-locked.
- Save/load does not replay completed one-shot scares.
- Audio and camera effects fit the scene pacing.
