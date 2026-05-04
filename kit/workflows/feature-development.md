# UHFPS Steam Horror Feature Development Workflow

## Goal

Develop UHFPS-compatible Steam horror features without breaking UHFPS core
systems, Zenject composition, save/load behavior, or PC horror pacing.

## Step 1 - Feature Brief

```text
Feature:
Player Experience:
UHFPS Systems:
Persistent State:
Scene/Prefab Assets:
Horror Intent:
Constraints:
```

## Step 2 - Architecture

Use `agents/uhfps-architect.md`.

Define UHFPS boundaries, Zenject services, Inspector references, save/load
impact, setup, risks, and validation.

## Step 3 - Implementation

Use `agents/uhfps-gameplay-programmer.md`.

Implement only the scoped feature. Do not rewrite UHFPS core systems or create
parallel systems.

## Step 4 - Setup

Use `agents/uhfps-setup.md`.

Define scene, prefab, Inspector, UHFPS asset, Zenject, and save/load setup.

## Step 5 - Review

Use:

- `agents/code-reviewer.md`
- `agents/save-load-reviewer.md` when persistent state changes
- `agents/horror-design-reviewer.md` when player experience is affected

## Step 6 - Validate

Use `commands/validate.md`.

Do not mark complete without explicit evidence or clearly stated unrun checks.
