# UHFPS Inventory Skill

## Purpose

Use this skill when adding inventory items, item selectors, item usage, or item
driven puzzle logic.

## Core Idea

Use the UHFPS Inventory Database and inventory item flows. Do not create a
parallel inventory.

## Key UHFPS Types

- `Inventory`
- `InventoryItem`
- `Item`
- `ItemGuid`
- `ItemProperty`
- `ItemCustomData`
- `InventoryUseEvents`
- `ItemUseEvent`
- `IInventorySelector`
- `InteractableItem`

## Important Inventory APIs

Use the documented `Inventory.Instance` API when integrating with UHFPS
inventory. This is an official UHFPS singleton, not a project-created service
locator.

Common operations:

- `Inventory.Instance.ContainsItem(string guid)`
- `Inventory.Instance.ContainsItem(string guid, out OccupyData data)`
- `Inventory.Instance.ContainsItemMany(string guid, out OccupyData[] data)`
- `Inventory.Instance.GetInventoryItem(string guid)`
- `Inventory.Instance.GetItemQuantity(string guid)`
- `Inventory.Instance.GetAllItemsQuantity(string guid)`
- `Inventory.Instance.AddItem(string guid, ushort quantity, ItemCustomData customData)`
- `Inventory.Instance.RemoveItem(string guid)`
- `Inventory.Instance.RemoveItem(InventoryItem item)`
- `Inventory.Instance.RemoveItem(InventoryItem item, ushort quantity)`
- `Inventory.Instance.RemoveItemQuantityMany(string guid, ushort quantity)`
- `Inventory.Instance.OpenItemSelector(IInventorySelector selector)`
- `Inventory.Instance.RegisterUseEvent(string itemGuid, Action<ItemUseEvent> evt)`

## Item Usage

Use `InventoryUseEvents` for simple designer-driven item use events.

Use `RegisterUseEvent` when code must react to a specific item use:

```csharp
using UnityEngine;
using UHFPS.Runtime;

public sealed class FuseInventoryUse : MonoBehaviour
{
    [SerializeField] private ItemGuid fuseItem;
    [SerializeField] private FuseboxPuzzle puzzle;

    private void Awake()
    {
        if (puzzle == null)
            throw new System.InvalidOperationException(
                "FuseInventoryUse: FuseboxPuzzle reference is missing. Assign it in the Inspector.");
    }

    private void Start()
    {
        Inventory.Instance.RegisterUseEvent(fuseItem, OnFuseUsed);
    }

    private void OnFuseUsed(ItemUseEvent evt)
    {
        puzzle.TryInsertFuse(evt.Item);
    }
}
```

## Inventory Selectors

Use `IInventorySelector` when the player must choose an item for a lock or
puzzle. UHFPS calls:

```csharp
void OnInventoryItemSelect(Inventory inventory, InventoryItem selectedItem)
```

This is the right pattern for key locks, broken object repair, fuse insertion,
and item-driven dynamic unlocks.

## Interactable Pickups

Use `InteractableItem` for world pickups. Important fields:

- `InteractableType`: `GenericItem`, `InventoryItem`, `ExamineItem`, `InventoryExpand`
- `PickupItem`
- `ItemCustomData`
- `Quantity`
- `AutoShortcut`
- `AutoEquip`
- `UseInventoryTitle`
- `ExamineInventoryTitle`
- `DisableType`: `None`, `Deactivate`, `Destroy`
- `OnTakeEvent`

`InteractableItem` derives from `SaveableBehaviour`, so it already saves
position, rotation, quantity, active state, hotspot state, and custom data.

## Rules

- Do not hardcode item discovery through scene search.
- Use `ItemGuid` or UHFPS item references where the framework expects them.
- Keep item requirement logic explicit and testable.
- Review save/load when items are consumed, dropped, unlocked, or spawned.
- Do not create a parallel item database.
- Do not remove an item before confirming the action actually succeeds.
- When consuming multiple stacked items, prefer quantity-aware removal methods.

## Setup Checklist

- Inventory Database updated
- item icon configured as Sprite when needed
- item dimensions configured
- item assigned to pickup/interactable object
- GAMEMANAGER inventory reference confirmed
- item use/equip behavior connected through UHFPS flow
- `InventoryUseEvents` added when designer-driven item use is enough
- `IInventorySelector` script assigned when item choice is required
- item custom JSON reviewed if used for puzzle-specific state
- save/load reviewed for consumed, dropped, spawned, or quantity-changing items

## Validation

- Pickup adds the correct item and quantity.
- Inventory UI shows correct title, size, icon, and custom behavior.
- Item use does not fire when inventory, pause, or examine state should block it.
- Wrong selected item gives clear feedback and does not consume anything.
- Save/load preserves inventory, dropped items, and consumed items.
