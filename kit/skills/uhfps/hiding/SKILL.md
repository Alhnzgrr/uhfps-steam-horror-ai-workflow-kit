# UHFPS Hiding Skill

## Purpose

Use this skill when adding hiding spots, lockers, under-bed hiding, enemy
avoidance beats, or interactions that move the player into a protected state.

## Key UHFPS Types

- `HideInteract`
- `PlayerPresenceManager`
- player state machine hiding states
- AI states that react to player hiding
- `IInteractStart`

## Core Idea

Use UHFPS hiding and player state flow. Do not create a separate stealth state
that bypasses the UHFPS player state machine.

## Rules

- Do not teleport or freeze the player without restoring control through UHFPS flow.
- Do not make hiding a guaranteed safe state unless the design requires it.
- Do not break enemy perception or pursuit state by bypassing UHFPS AI hooks.
- Save/load any persistent hiding spot occupancy or blocked state.

## Setup Checklist

- hiding interact component attached
- object layer set to `Interact`
- enter and exit transforms configured
- camera/player transition tested
- enemy behavior around hiding reviewed
- audio/animation feedback assigned
- save/load reviewed if hiding changes world state

## Validation

- Player enters and exits hiding cleanly.
- Pause/inventory/cutscene cannot corrupt hiding state.
- Enemy behavior remains coherent.
- Horror pacing preserves vulnerability and uncertainty.
