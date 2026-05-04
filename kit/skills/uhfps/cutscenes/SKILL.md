# UHFPS Cutscenes Skill

## Purpose

Use this skill when adding Timeline cutscenes, player-camera cutscenes,
Cinemachine transitions, or event-triggered narrative sequences.

## Key UHFPS Types

- `CutsceneTrigger`
- `CutscenePlayer`
- `CutsceneModule`
- `PlayableDirector`
- `CinemachineCamera`
- `CinemachineBlendDefinition`
- `DialogueSystem`
- `PlayerPresenceManager`

## CutsceneTrigger Configuration

Important enums:

- `TriggerType`: `Trigger`, `Interact`, `Event`
- `CutsceneType`: `CameraCutscene`, `PlayerCutscene`

Important fields:

- `Cutscene`
- `CutscenePlayer`
- `CutsceneCamera`
- `CutsceneFadeSpeed`
- `BlendDefinition`
- `CustomBlendAsset`
- `WaitForDialogue`
- `WaitForBlendIn`
- `BlendInOffset`
- `BlendOutTime`
- `CutEndTransform`
- `CutFadeInSpeed`
- `CutFadeOutSpeed`
- `OnCutsceneStart`
- `OnCutsceneEnd`

`CutsceneTrigger` implements `IInteractStart` and `ISaveable`; it saves whether
the cutscene has already played.

## Trigger Flow

Use:

```csharp
cutsceneTrigger.TriggerCutscene();
```

for event-driven playback.

Do not manually freeze the player and switch cameras outside UHFPS cutscene
flow unless the feature is intentionally custom and reviewed.

## Player Cutscene Setup

`CutscenePlayer` has a `HeadCamera` and can toggle it through
`SetCutsceneActive(bool)`.

Use `CutEndTransform` when the player should end at a specific position after a
cutscene. Validate the gizmo capsule against the player character controller.

## Rules

- Do not start a cutscene while dialogue is playing if `WaitForDialogue` is required.
- Do not leave player controls frozen after the cutscene.
- Do not bypass UHFPS camera switching with raw camera activation unless reviewed.
- Do not replay one-shot story cutscenes after save/load.

## Setup Checklist

- PlayableDirector assigned
- CutsceneModule exists in GameManager modules
- cutscene camera assigned
- blend settings reviewed
- trigger collider or interact setup configured
- `OnCutsceneStart` and `OnCutsceneEnd` events connected
- save/load one-shot behavior reviewed
- player end transform assigned when needed

## Validation

- Trigger, interact, or event start works.
- Dialogue gating works.
- Camera blends in and out cleanly.
- Player returns to correct state after cutscene.
- Save/load does not replay completed cutscenes unless intended.
