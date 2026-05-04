# UHFPS Puzzles Skill

## Purpose

Use this skill when adding examine-camera puzzles, pointer puzzles, keypad/safe
style puzzles, or puzzle objects that switch the player into a focused state.

## Key UHFPS Types

- `PuzzleBase`
- `PuzzleBaseSimple`
- `PuzzleBaseBlend`
- `PuzzleExamine`
- built-in puzzle examples such as keypad, safe, padlock, fusebox, levers,
  electrical circuit, maze, keycard, and lockpick
- `GameManager`
- `PlayerPresenceManager`
- `ControlsContext`

## PuzzleBase Flow

`PuzzleBase` implements `IInteractStart`.

On interact it:

- freezes the player
- disables player item usability
- switches to the puzzle camera
- optionally shows pointer controls
- toggles configured colliders
- shows control info

It switches back through the examine input when allowed.

Important fields:

- `PuzzleCamera`
- `SwitchCameraFadeSpeed`
- `ControlsContexts`
- `CullLayers`
- `InteractLayer`
- `DisabledLayer`
- `EnablePointer`
- `CollidersEnable`
- `CollidersDisable`
- `OnScreenFade`

## Rules

- Do not create a new puzzle camera flow when `PuzzleBase` fits.
- Do not leave player frozen or player items disabled after exit.
- Do not put core puzzle state only in UI visuals.
- Do not make puzzle prompts too explicit for horror pacing.
- Save solved/partial state when the puzzle changes world progression.

## Setup Checklist

- puzzle object layer set to `Interact`
- puzzle camera assigned
- controls contexts localized or plain-text intentional
- pointer cull layers and interact layer configured
- colliders enabled/disabled lists configured
- exit input tested
- objective/dynamic object/inventory consequences wired
- save/load state reviewed

## Validation

- Interact enters puzzle state.
- Player cannot move/use items during puzzle.
- Pointer hits only intended puzzle interactables.
- Exit returns camera, controls, UI, and item usability.
- Save/load preserves solved state and world consequences.
