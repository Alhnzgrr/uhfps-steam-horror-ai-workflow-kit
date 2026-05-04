# PC Input And Build Rules

## Core Rule

The target platform is Steam PC unless the project says otherwise.

Do not design or validate features with mobile-first assumptions.

## Required Support

Features must work with:

- keyboard
- mouse
- standalone builds
- pause/menu flow
- UHFPS input flow

## Forbidden

- mobile UX patterns as the default
- touch-only interactions
- assumptions about portrait layout
- breaking cursor lock, pause, inventory, or menu behavior
- bypassing UHFPS input handling with unrelated parallel input systems

## Input Integration

Use UHFPS input and interaction flows where applicable.

Custom input code must:

- stay outside core rule logic
- forward intent to systems or UHFPS-compatible components
- not duplicate UHFPS controls without a strong reason

## Validation Questions

- Does it work with keyboard and mouse?
- Does it behave correctly when paused?
- Does it conflict with inventory, examine, or interaction modes?
- Does it work in a standalone build, not only in Editor?
