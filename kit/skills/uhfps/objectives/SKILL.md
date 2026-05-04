# UHFPS Objectives Skill

## Purpose

Use this skill when adding objective progression, sub-objectives, objective
triggers, or puzzle completion flow.

## Core Idea

Use UHFPS Objectives assets and Objective Manager flow. Do not create a separate
quest or objective system.

## Key UHFPS Types

- `ObjectiveManager`
- `ObjectiveTrigger`
- `ObjectiveEvent`
- `Objective`
- `SubObjective`
- `ObjectiveSelect`
- `SingleObjectiveSelect`
- `ObjectivesAsset`

## ObjectiveManager API

Use documented UHFPS objective calls:

- `ObjectiveManager.Instance.AddObjective(string key, params string[] subKey)`
- `ObjectiveManager.Instance.AddSubObjective(string key, string[] subKey)`
- `ObjectiveManager.Instance.CompleteObjective(string key, params string[] subKey)`
- `ObjectiveManager.Instance.DiscardObjective(string key, params string[] subKey)`

`ObjectiveManager` saves active objectives through `ISaveableCustom`.

## ObjectiveTrigger Setup

`ObjectiveTrigger` supports:

- `TriggerType`: `Trigger`, `Interact`, `Event`
- `ObjectiveType`: `New`, `Complete`, `NewAndComplete`
- `objectiveToAdd`
- `objectiveToComplete`

It implements `IInteractStart` and `ISaveable`.

For event-driven activation, call:

```csharp
objectiveTrigger.TriggerObjective();
```

For code-driven activation without a trigger component:

```csharp
ObjectiveManager.Instance.CompleteObjective("restore_power", "insert_fuse");
```

## ObjectiveEvent

Use `ObjectiveEvent` to connect UnityEvents to:

- objective added
- objective completed
- sub-objective added
- sub-objective completed
- sub-objective count changed

This is the clean hook for doors, lights, sounds, or narrative beats that react
to objective progress.

## Text and Count

Use sub-objective complete count for repeated goals. Use `[count]` in objective
text when progress should be displayed.

In horror pacing, avoid objective text that over-explains the solution. Prefer
concrete intent over puzzle spoilers.

## Rules

- Do not hardcode objective UI separately from UHFPS objectives.
- Trigger objectives through UHFPS objective flow.
- Keep objective text clear but not overly explanatory for horror pacing.
- Review save/load for objective completion state.
- Do not invent parallel quest state.
- Do not complete objectives before the authoritative action succeeds.
- Use stable keys and do not rename keys casually after content is authored.

## Setup Checklist

- Objectives asset created or updated
- sub-objectives configured
- Objective Manager has the correct asset
- Objective Trigger component attached when needed
- trigger type selected
- event call documented if using event activation
- ObjectiveEvent added if other scene objects react to objective state
- completion count and `[count]` text reviewed
- objective notification pacing reviewed
- save/load test includes active and completed objective states

## Validation

- Objective appears when expected.
- Sub-objective count increments correctly.
- Completion event fires once at the right time.
- Save/load restores active objectives and counts.
- Objective text supports tension instead of solving the puzzle for the player.
