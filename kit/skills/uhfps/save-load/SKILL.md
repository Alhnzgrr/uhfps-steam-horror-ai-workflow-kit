# UHFPS Save Load Skill

## Purpose

Use this skill when a feature affects persistent state, runtime-spawned objects,
inventory state, objectives, locks, puzzles, or world changes.

## Core Idea

Use UHFPS save/load flows and restore state deterministically. Do not create a
parallel save system.

## UHFPS Concepts

- `ISaveable` handles scene component save data.
- `SaveableBehaviour` supports runtime saveable objects.
- Saveable discovery generates references that must be persisted with the scene.
- Runtime saveable prefab references must be registered through UHFPS object
  reference flows.

## Rules

- Do not rely on runtime discovery during load.
- Do not save only visuals.
- Do not ignore dropped, consumed, or spawned item state.
- Always define what happens after scene reload and save reload.

## Setup Checklist

- saveable interface or base class selected
- saved fields listed
- loaded fields listed
- scene saveable discovery required or not required
- object references configured for runtime spawned prefabs
- manual load test defined
