# UHFPS Steam Horror AI Workflow Kit - AGENTS.md

## Overview

This file defines the role and routing system for AI-assisted Unity development
in Steam horror projects that use:

- UHFPS (Ultimate Horror FPS Kit)
- Zenject

Use this file as the primary context entry point.

## Core Position

UHFPS is the base gameplay framework.

The AI must not rewrite, replace, or duplicate UHFPS core systems. The AI should
extend UHFPS through documented APIs, explicit scene setup, custom
MonoBehaviours, adapters, and Zenject-managed gameplay services.

## Agent Roster

| Agent | File | Responsibility |
|---|---|---|
| UHFPS Architect | `agents/uhfps-architect.md` | Feature design, UHFPS boundaries, Zenject split |
| UHFPS Gameplay Programmer | `agents/uhfps-gameplay-programmer.md` | Feature implementation without core rewrites |
| UHFPS Setup Specialist | `agents/uhfps-setup.md` | Scene, prefab, Inspector, and UHFPS asset wiring |
| Horror Design Reviewer | `agents/horror-design-reviewer.md` | Tension, pacing, exploration, Steam horror quality |
| Save Load Reviewer | `agents/save-load-reviewer.md` | UHFPS save/load safety and deterministic restore |
| Code Reviewer | `agents/code-reviewer.md` | Correctness, architecture, lifecycle, integration risks |

## Standard Feature Flow

For non-trivial features:

1. Design with UHFPS Architect.
2. Implement with UHFPS Gameplay Programmer.
3. Define setup with UHFPS Setup Specialist.
4. Review save/load if state changes.
5. Review code.
6. Review horror design if the feature affects player experience.
7. Validate with explicit evidence.

## Quick Flow

For small, low-risk additions:

1. UHFPS Gameplay Programmer
2. UHFPS Setup Specialist if scene or prefab wiring is involved
3. Code Reviewer

## Decision Gates

| Trigger | Required files |
|---|---|
| Any UHFPS feature | `rules/uhfps-core-protection.md` |
| Zenject service or system | `rules/zenject-integration.md` |
| Scene, prefab, or component reference | `rules/runtime-reference-safety.md` |
| Persistent game state | `rules/uhfps-save-load.md` |
| Player-facing horror beat | `rules/steam-horror-design.md` |
| Keyboard, mouse, pause, or build concern | `rules/pc-input-build.md` |

## UHFPS Skill Routing

| Work area | Skill |
|---|---|
| Interactable objects | `skills/uhfps/interactions/SKILL.md` |
| Inventory items or selectors | `skills/uhfps/inventory/SKILL.md` |
| Doors, drawers, locks, levers | `skills/uhfps/dynamic-objects/SKILL.md` |
| Flashlight, weapons, held tools | `skills/uhfps/player-items/SKILL.md` |
| Objectives and progression prompts | `skills/uhfps/objectives/SKILL.md` |
| Save/load and runtime spawned objects | `skills/uhfps/save-load/SKILL.md` |
| Game Manager modules and UI references | `skills/uhfps/game-manager/SKILL.md` |

## Non-Negotiable Behavior

- Do not use scene search as dependency resolution.
- Do not auto-fix missing setup.
- Do not hide errors behind logs and fallback logic.
- Do not rewrite UHFPS core systems.
- Do not build parallel systems that duplicate UHFPS features.
- Do not assume mobile UX.
- Do not claim a feature is complete without setup and validation notes.

## UHFPS API Exception

Documented UHFPS APIs, including documented singleton entry points, may be used
when the official UHFPS flow requires them.

This is not permission to invent hidden singleton access or search the scene.
When using a UHFPS singleton/API, state why it is the documented integration
point and keep custom project logic outside the UHFPS core.

## Required Pre-Code Explanation

Before writing implementation code for a feature, state:

1. Which UHFPS system is being used.
2. Which scene objects, prefabs, or assets are involved.
3. What new scripts will be created.
4. What existing UHFPS systems are referenced.
5. Whether save/load integration is required.
6. Which dependencies come from Zenject and which references come from Inspector.

## Command Reference

- `commands/architect.md`
- `commands/build-feature.md`
- `commands/review-code.md`
- `commands/validate.md`
- `commands/catch-up.md`

## Templates

- `templates/architecture-handoff.md`
- `templates/implementation-handoff.md`
- `templates/setup-handoff.md`
- `templates/review-handoff.md`
- `templates/validation-report.md`
