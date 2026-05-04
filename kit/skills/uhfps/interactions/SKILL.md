# UHFPS Interactions Skill

## Purpose

Use this skill when creating or reviewing interactable objects in UHFPS.

## Core Idea

Use UHFPS interaction layers, components, events, and interfaces instead of
building a parallel interaction system.

## UHFPS Concepts

- Interactable objects use the `Interact` layer.
- Custom behavior can use `Custom Interact Event`.
- Custom scripts can implement UHFPS interaction interfaces.
- Common interfaces include hover, interact start/hold/stop, timed interact,
  title override, and examine interaction variants.

## Rules

- Do not search for the player or interaction manager.
- Do not duplicate the UHFPS interaction raycast/input flow.
- Use explicit component references for connected objects.
- For timed interactions, expose timing in Inspector and validate state clearly.

## Setup Checklist

- object layer set to `Interact`
- required UHFPS interaction component attached
- custom script implements the correct UHFPS interface
- interact title and reticle behavior defined
- invalid interaction feedback defined
- save/load impact reviewed if interaction changes persistent state
