# UHFPS Player Items Skill

## Purpose

Use this skill when adding held tools, flashlight-like items, weapons, or other
player equipment.

## Core Idea

Use UHFPS Player Items and `PlayerItemBehaviour`. Do not create a parallel equip
or hands system.

## Key UHFPS Types

- `PlayerItemsManager`
- `PlayerItemBehaviour`
- `InventoryItem`
- `ItemGuid`
- `MotionPreset`
- `ExternalMotions`
- `MotionController`

## PlayerItemBehaviour Contract

Custom player items derive from `PlayerItemBehaviour` and implement:

- `OnItemSelect()`
- `OnItemDeselect()`
- `OnItemActivate()`
- `OnItemDeactivate()`

Optional overrides:

- `Name`
- `IsBusy()`
- `IsEquipped()`
- `CanCombine()`
- `OnUpdate()`
- `OnItemCombine(InventoryItem combineItem)`
- `OnCustomSave()`
- `OnCustomLoad(JToken data)`

Important inherited references/settings:

- `ItemObject`
- `MotionPivot`
- `EnableWallDetection`
- `WallHitTransform`
- `WallHitMask`
- `MotionPreset`
- `ExternalMotions`
- `ApplyEffect(string eventID)`
- `PlayerItems`
- `CanInteract`

UHFPS base class internally resolves related player components from the
HEROPLAYER hierarchy. Do not duplicate that lookup in custom scripts.

## Minimal Custom Item Pattern

```csharp
using Newtonsoft.Json.Linq;
using UnityEngine;
using UHFPS.Runtime;

public sealed class UVLampItem : PlayerItemBehaviour
{
    [SerializeField] private Light uvLight;

    private bool active;

    public override string Name => "UV Lamp";
    public override bool IsBusy() => false;

    private void Awake()
    {
        if (uvLight == null)
            throw new System.InvalidOperationException(
                "UVLampItem: UV Light reference is missing. Assign it in the Inspector.");
    }

    public override void OnItemSelect()
    {
        ItemObject.SetActive(true);
        uvLight.enabled = active;
    }

    public override void OnItemDeselect()
    {
        uvLight.enabled = false;
        ItemObject.SetActive(false);
    }

    public override void OnItemActivate()
    {
        active = true;
        ItemObject.SetActive(true);
        uvLight.enabled = true;
    }

    public override void OnItemDeactivate()
    {
        active = false;
        uvLight.enabled = false;
    }

    public override StorableCollection OnCustomSave()
    {
        return new StorableCollection { { nameof(active), active } };
    }

    public override void OnCustomLoad(JToken data)
    {
        active = (bool)data[nameof(active)];
    }
}
```

## Inventory Integration

To make an item equippable:

- create the item in the Inventory Database
- enable usable settings
- choose Player Item usable type
- assign the Player Item reference/index expected by UHFPS
- add the `PlayerItemBehaviour` component to `PlayerItemsManager.PlayerItems`

Use `OnItemCombine(InventoryItem combineItem)` for battery/ammo/refill style
flows. Remove inventory quantities only after the combine action is accepted.

## UI References

Some built-in items, such as flashlight, read UI from
`GameManager.GraphicReferences.Value["Flashlight"]`.

For custom item UI:

- add a named custom UI reference in GAMEMANAGER
- document expected array order and component types
- validate at runtime with descriptive errors before use

## Save/Load

Use `OnCustomSave` and `OnCustomLoad` for item state such as:

- battery energy
- ammo loaded
- current mode
- durability
- whether a light/tool is active
- cooldowns that must persist

## Rules

- Do not replace the player item manager.
- Keep item-specific logic in the custom item script.
- Use Inspector references for Animator, item object, audio, light, and effects.
- Keep PC input and pause/menu behavior intact.
- Do not directly poll input in the item if UHFPS item activation already handles it.
- Do not activate item effects when `CanInteract` is false.
- Do not leave light/audio/VFX active after deselect/deactivate.

## Setup Checklist

- item object created under the correct HEROPLAYER hierarchy
- Animator Controller assigned if needed
- custom `PlayerItemBehaviour` script attached
- Player Items Manager updated
- inventory usable item linked when applicable
- wall detection/motion options reviewed
- save/load reviewed for batteries, ammo, durability, or item state
- `ItemObject` assigned
- `MotionPivot` assigned or intentionally left to camera hands pivot
- item model/animator child active state verified
- external motion events added if using `ApplyEffect`
- custom UI references added to GAMEMANAGER if needed

## Validation

- Select/deselect transitions cannot be spammed into broken state.
- Item hides on walls if wall detection is enabled.
- Pause, inventory, examine, and cutscene states block item use correctly.
- Save/load restores item state without turning effects on unexpectedly.
