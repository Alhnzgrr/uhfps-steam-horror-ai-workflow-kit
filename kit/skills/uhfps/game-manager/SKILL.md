# UHFPS Game Manager Skill

## Purpose

Use this skill when integrating with GAMEMANAGER, custom UI references, manager
modules, or global UHFPS runtime services.

## Core Idea

Respect UHFPS Game Manager ownership. Use documented access points and explicit
setup without creating a competing global manager.

## Key UHFPS Types

- `GameManager`
- `PlayerPresenceManager`
- `ManagerModule`
- `ManagerModulesAsset`
- `Inventory`
- `ObjectiveManager`
- `DialogueSystem`

## GameManager Responsibilities

GAMEMANAGER owns high-level UHFPS runtime state:

- panels and UI groups
- pause and inventory flow
- reticle and pointer
- hint and paper UI
- background fade and blur
- player presence
- custom graphic references
- manager modules

Common documented APIs:

- `GameManager.Instance.ShowHintMessage(string text, float time)`
- `GameManager.Instance.ShowItemPickupMessage(string text, Sprite icon, float time)`
- `GameManager.Instance.FreezePlayer(bool state, bool showCursor = false, bool lockInput = true)`
- `GameManager.Instance.LockInput(bool state)`
- `GameManager.Instance.ShowPanel(GameManager.PanelType panel)`
- `GameManager.Instance.ShowInventoryPanel(bool state)`
- `GameManager.Instance.StartBackgroundFade(...)`
- `GameManager.SubscribePauseEvent(Action<bool>)`
- `GameManager.SubscribeInventoryEvent(Action<bool>)`
- `GameManager.Module<T>()`

## Custom Graphic References

Use GAMEMANAGER custom UI references for player item or feature UI that must be
globally accessible through UHFPS.

Pattern:

```csharp
var behaviours = GameManager.Instance.GraphicReferences.Value["Flashlight"];
CanvasGroup panel = (CanvasGroup)behaviours[0];
```

When using this pattern in custom code:

- document the reference name
- document array order
- validate key existence and type casts
- keep UI feature-specific, not a general service locator

## Manager Modules

`ManagerModule` is a serializable UHFPS module stored in `ManagerModulesAsset`.

It supports:

- `Name`
- `OnAwake()`
- `OnStart()`
- `OnUpdate()`
- `RunCoroutine(IEnumerator)`
- protected access to `Inventory` and `PlayerPresence`

Use manager modules for UHFPS-level behavior that truly belongs to GAMEMANAGER.
Do not use them for scene objects or feature services better handled by Zenject.

## PlayerPresenceManager

Use documented player presence APIs for camera/player state:

- `UnlockPlayer()`
- `FreezePlayer(bool, bool showCursor = false)`
- `FreezeMovement(bool)`
- `FreezeLook(bool, bool showCursor = false)`
- `SwitchActiveCamera(...)`
- `SwitchToPlayerCamera(...)`
- `Teleport(...)`
- `SetPlayerTransform(...)`
- `SetPlayerPositionAndLook(...)`

Player rotation is intentionally reset to identity while look rotation is stored
in the look controller. Preserve this UHFPS behavior.

## Rules

- Do not replace GAMEMANAGER.
- Do not add unrelated responsibilities to a custom global manager.
- Do not use Game Manager access as a general service locator for project code.
- Wrap UHFPS access in small adapters when custom services need testable boundaries.
- Do not break pause, inventory, pointer, examine, or cutscene input gates.
- Do not register manager modules just to avoid Inspector setup or Zenject bindings.
- Do not store scene references inside `ManagerModule` ScriptableObject-like data.

## Setup Checklist

- required custom UI references added
- manager module need justified
- module has no scene references if ScriptableObject-based
- Zenject service boundary defined when custom systems depend on Game Manager data
- pause/inventory subscriptions disposed or owned by UHFPS when using static subscribe helpers
- UI reference name and index order documented
- player freeze/unfreeze balanced on every path
- cursor state reviewed when freezing look/player

## Validation

- Pause still opens/closes correctly.
- Inventory still opens/closes correctly.
- Player is unfrozen after custom interaction/cutscene/jumpscare.
- Custom UI reference lookup fails clearly if setup is missing.
