# UHFPS Interactions Skill

## Purpose

Use this skill when creating or reviewing interactable objects in UHFPS.

## Core Idea

Use UHFPS interaction layers, components, events, and interfaces instead of
building a parallel interaction system.

## Key UHFPS Types

- `IHoverStart` / `IHoverEnd`
- `IInteractStart`
- `IInteractHold(Vector3 point)`
- `IInteractStop`
- `IInteractStartPlayer(GameObject player)`
- `IInteractTimed`
- `IStateInteract`
- `IInteractTitle`
- `IExamineClick`
- `IExamineDragVertical`
- `IExamineDragHorizontal`
- `CustomInteractEvent`
- `CustomInteractTitle`
- `CustomInteractReticle`
- `TimedInteractEvent`
- `InteractableItem`

## Interface Behavior

Use the smallest interface that matches the desired behavior:

- `IInteractStart` for one-shot activation.
- `IInteractHold` for hold/drag style interactions that need hit point data.
- `IInteractStop` for cleanup after button release.
- `IInteractTimed` for hold-to-complete interactions. It requires:
  - `float InteractTime { get; set; }`
  - `bool NoInteract { get; }`
  - `void InteractTimed()`
- `IInteractTitle` to override prompt title/buttons through `TitleParams`.
- `IStateInteract` when interaction should request a player state transition.
- `IExamine*` interfaces for object-specific examine mode interactions.

## Built-In Components

`CustomInteractEvent` is useful for designer-driven events. It supports:

- start, hold, and stop events
- optional player freeze
- interact-once behavior
- interact sound
- `ISaveable` state for interact-once persistence

`TimedInteractEvent` is useful for hold interactions. It supports:

- `InteractTime`
- interact once or reset behavior
- required inventory item through `ItemGuid`
- optional hint through `GameManager.Instance.ShowHintMessage`
- `ISaveable` state for completed/reset state

`CustomInteractTitle` supports localized static or reflection-driven titles.
If dynamic title is enabled, the reflected member must resolve to a bool.

`CustomInteractReticle` implements `IReticleProvider` and can switch reticle
state dynamically during hold interactions.

`InteractableItem` covers generic pickup, inventory item pickup, examine items,
inventory expansion, hotspot behavior, pickup messages, custom data, and
runtime save/load through `SaveableBehaviour`.

## Implementation Pattern

```csharp
using UnityEngine;
using UHFPS.Runtime;

public sealed class GeneratorSwitch : MonoBehaviour, IInteractStart, IInteractTitle
{
    [SerializeField] private GeneratorPuzzle generatorPuzzle;

    private void Awake()
    {
        if (generatorPuzzle == null)
            throw new System.InvalidOperationException(
                "GeneratorSwitch: GeneratorPuzzle is missing. Assign it in the Inspector.");
    }

    public void InteractStart()
    {
        generatorPuzzle.TogglePower();
    }

    public TitleParams InteractTitle()
    {
        return new TitleParams
        {
            title = "*Generator",
            button1 = "*Toggle",
            button2 = null
        };
    }
}
```

Use a leading `*` in visible text when the text should be treated as literal
plain text instead of a localization key.

## Rules

- Do not search for the player or interaction manager.
- Do not duplicate the UHFPS interaction raycast/input flow.
- Use explicit component references for connected objects.
- For timed interactions, expose timing in Inspector and validate state clearly.
- Use documented UHFPS singleton APIs only when matching existing UHFPS flow.
- Add `ISaveable` only when interaction state must persist.

## Setup Checklist

- object layer set to `Interact`
- required UHFPS interaction component attached
- custom script implements the correct UHFPS interface
- interact title and reticle behavior defined
- invalid interaction feedback defined
- save/load impact reviewed if interaction changes persistent state
- object collider present and queryable by UHFPS interaction raycast
- examine colliders enabled/disabled intentionally when using examine mode
- `InteractableItem.DisableType` chosen when pickup should deactivate/destroy
- `OnTakeEvent`, `OnExamineStartEvent`, and `OnExamineEndEvent` connected when needed

## Validation

- Hover prompt appears.
- Use input triggers the correct interface method once.
- Hold/timed progress behaves correctly and stops on release.
- Invalid state produces a hint, locked sound, or clear non-comedic feedback.
- Scene reload and save/load preserve interact-once or pickup state when applicable.
