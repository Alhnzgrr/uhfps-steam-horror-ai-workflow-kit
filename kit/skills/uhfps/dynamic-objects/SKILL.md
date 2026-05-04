# UHFPS Dynamic Objects Skill

## Purpose

Use this skill for doors, drawers, levers, valves, locks, and other UHFPS dynamic
objects.

## Core Idea

Use UHFPS `DynamicObject` and its unlock/interaction extension points instead of
building a separate door or lock system.

## Key UHFPS Types

- `DynamicObject`
- `DynamicObjectType`
- `DynamicOpenable`
- `DynamicPullable`
- `DynamicSwitchable`
- `DynamicRotable`
- `IDynamicUnlock`
- `IInventorySelector`

## DynamicObject Configuration

Important enums:

- `DynamicType`: `Openable`, `Pullable`, `Switchable`, `Rotable`
- `TransformType`: `Local`, `Global`
- `InteractType`: `Dynamic`, `Mouse`, `Animation`
- `DynamicStatus`: `Normal`, `Locked`
- `StatusChange`: `InventoryItem`, `CustomScript`, `None`

Important references:

- `target`
- `audioSource`
- `animator`
- `joint`
- `rigidbody`
- `unlockScript`
- `unlockItem`
- `ignoreColliders`

Use `SetOpenState()` and `SetCloseState()` for event-driven open/close when the
object is not mouse-driven. Use `SetLockedStatus(bool)` for state changes and
`TryUnlockResult(bool)` from custom unlock scripts.

## Unlock Patterns

For simple key/item locks, use:

- `dynamicStatus = Locked`
- `statusChange = InventoryItem`
- `unlockItem = required ItemGuid`
- `keepUnlockItem` depending on whether the key should be consumed

For custom logic, assign a script implementing `IDynamicUnlock`:

```csharp
using UnityEngine;
using UHFPS.Runtime;

public sealed class WardDoorUnlock : MonoBehaviour, IDynamicUnlock, IInventorySelector
{
    [SerializeField] private ItemGuid requiredKey;

    private DynamicObject pendingDoor;

    public void OnTryUnlock(DynamicObject dynamicObject)
    {
        pendingDoor = dynamicObject;
        Inventory.Instance.OpenItemSelector(this);
    }

    public void OnInventoryItemSelect(Inventory inventory, InventoryItem selectedItem)
    {
        bool unlocked = selectedItem.ItemGuid == requiredKey;
        if (unlocked)
            inventory.RemoveItem(selectedItem);

        pendingDoor.TryUnlockResult(unlocked);
    }
}
```

## Save/Load

`DynamicObject` implements `ISaveable`.

It saves:

- locked state
- openable rotation/open values
- pullable/switchable/rotable state through the selected dynamic type

When custom unlock scripts have additional state, add `ISaveable` to the custom
script too.

## Rules

- Do not animate or unlock persistent objects outside the UHFPS dynamic object flow.
- Do not search for inventory or dynamic object dependencies.
- Use explicit references and UHFPS callbacks.
- Review save/load for open/closed/locked/unlocked state.
- Do not bypass `TryUnlockResult` in custom unlock scripts.
- Do not consume inventory items until unlock success is known.
- For mouse interaction, configure physics references instead of scripting drag logic separately.

## Setup Checklist

- Dynamic Object component attached
- target, hinge, axis, limits, and curves configured
- lock/unlock script assigned when needed
- required inventory item reference assigned
- gizmo-based setup verified
- save/load behavior reviewed
- object layer set to `Interact`
- collider present
- `target` points to the moving mesh/parent intentionally
- `audioSource` and sounds assigned when sound matters
- open/close/unlock/locked events wired
- `HingeJoint` and `Rigidbody` assigned for mouse openables
- animation triggers assigned for animation mode
- lock hint text localized or explicitly plain text

## Validation

- Door/drawer opens in correct direction from both sides if configured.
- Locked interaction plays locked feedback and does not change state.
- Correct item unlocks and consumes/keeps item according to setup.
- Wrong item does not unlock and does not consume item.
- Save/load restores open angle and locked state.
