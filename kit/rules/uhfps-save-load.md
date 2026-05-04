# UHFPS Save Load Rules

## Core Rule

If a feature changes persistent game state, it must define how that state is
saved, loaded, and restored deterministically.

## UHFPS Save Paths

Use UHFPS save/load concepts:

- `ISaveable` for scene object state.
- `SaveableBehaviour` for runtime saveable objects.
- UHFPS object references for runtime instantiated saveable prefabs.
- UHFPS Save Game Manager flows for runtime saveable registration.

## Required Questions

Before implementation, answer:

1. What state changes?
2. Does the state need to persist across save/load?
3. Is the object scene-authored or runtime-spawned?
4. Which UHFPS save interface or flow applies?
5. What must be restored on load?
6. What scene setup or GUID/object reference setup is required?

## Forbidden

- relying on scene search to restore state
- saving only visual state while authoritative state is lost
- runtime spawning persistent objects outside UHFPS saveable flow
- ignoring dropped inventory items or consumed puzzle items
- assuming reload will reconstruct state without evidence

## Setup Awareness

When scene saveables are added, setup notes must include:

- run UHFPS saveable discovery when required
- verify generated identifiers are stored
- save the scene after saveable discovery
- review removed saveables and stale references

## Review Questions

- Is persistent state explicit?
- Is restore deterministic?
- Does the feature use UHFPS save/load instead of a parallel save system?
- Are runtime spawned objects instantiated through the appropriate UHFPS flow?
