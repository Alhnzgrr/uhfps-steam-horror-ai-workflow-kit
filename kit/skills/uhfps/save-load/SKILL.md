# UHFPS Save Load Skill

## Purpose

Use this skill when a feature affects persistent state, runtime-spawned objects,
inventory state, objectives, locks, puzzles, or world changes.

## Core Idea

Use UHFPS save/load flows and restore state deterministically. Do not create a
parallel save system.

## Key UHFPS Types

- `ISaveable`
- `IRuntimeSaveable`
- `ISaveableCustom`
- `SaveableBehaviour`
- `SaveableObject`
- `SaveGameManager`
- `ObjectReference`
- `ObjectReferences`
- `StorableCollection`
- `UniqueID`

## Scene Saveables

Use `ISaveable` for scene-authored objects:

```csharp
using Newtonsoft.Json.Linq;
using UnityEngine;
using UHFPS.Runtime;

public sealed class GeneratorPuzzle : MonoBehaviour, ISaveable
{
    private bool powered;

    public StorableCollection OnSave()
    {
        return new StorableCollection { { nameof(powered), powered } };
    }

    public void OnLoad(JToken data)
    {
        powered = (bool)data[nameof(powered)];
        ApplyPoweredState();
    }
}
```

After adding scene `ISaveable` components, UHFPS saveable discovery must include
the component and the scene must be saved so generated tokens persist.

## Runtime Saveables

Use `SaveableBehaviour` for objects instantiated at runtime and expected to
persist.

Important requirement:

- the prefab must be registered in UHFPS Object References
- each runtime saveable component has a serialized `UniqueID`
- instantiate through `SaveGameManager.InstantiateSaveable(...)`

Use:

```csharp
GameObject obj = SaveGameManager.InstantiateSaveable(objectReference, position, rotation);
```

or:

```csharp
GameObject obj = SaveGameManager.InstantiateSaveable(referenceGuid, position, rotation);
```

Do not use plain `Instantiate` for persistent runtime objects.

## Built-In Helpers

`SaveableObject` can save common object state through flags:

- position
- rotation
- scale
- object active
- renderer active
- referenced behaviours active

Use this for simple prop state before writing a custom saveable.

`StorableCollection` is a string-keyed buffer. Use stable keys and keep load
code defensive enough to explain missing data during development.

## UHFPS Global Save

`SaveGameManager` also saves custom global data for:

- inventory
- objectives

Do not duplicate inventory or objective persistence.

## Rules

- Do not rely on runtime discovery during load.
- Do not save only visuals.
- Do not ignore dropped, consumed, or spawned item state.
- Always define what happens after scene reload and save reload.
- Do not use plain `Instantiate` for persistent dropped/spawned items.
- Do not rename save keys casually after saves may exist.
- Do not add a parallel JSON save system for feature state.
- Do not remove a runtime saveable without considering `SaveGameManager.RemoveSaveable`.

## Setup Checklist

- saveable interface or base class selected
- saved fields listed
- loaded fields listed
- scene saveable discovery required or not required
- object references configured for runtime spawned prefabs
- manual load test defined
- scene saved after saveable discovery
- removed saveables cleaned from Save Game Manager lists
- runtime prefab added to Object References
- generated `UniqueID` values checked on runtime saveable components
- inventory/objective persistence delegated to UHFPS

## Validation

- Save before interaction, load, state is unchanged.
- Save after interaction, load, state is restored.
- Runtime spawned object exists after load with correct component state.
- Removed/deactivated objects do not reappear incorrectly.
- Console has no missing token or missing UniqueID errors.
