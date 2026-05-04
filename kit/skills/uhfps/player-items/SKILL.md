# UHFPS Player Items Skill

## Purpose

Use this skill when adding held tools, flashlight-like items, weapons, or other
player equipment.

## Core Idea

Use UHFPS Player Items and `PlayerItemBehaviour`. Do not create a parallel equip
or hands system.

## UHFPS Concepts

- Player items are set up under the HEROPLAYER hands/FPView structure.
- Item behavior derives from `PlayerItemBehaviour`.
- Item select, deselect, activate, and deactivate are handled through overrides.
- Inventory can make items usable/equippable through player item references.
- Motion effects can be connected through UHFPS motion systems.

## Rules

- Do not replace the player item manager.
- Keep item-specific logic in the custom item script.
- Use Inspector references for Animator, item object, audio, light, and effects.
- Keep PC input and pause/menu behavior intact.

## Setup Checklist

- item object created under the correct HEROPLAYER hierarchy
- Animator Controller assigned if needed
- custom `PlayerItemBehaviour` script attached
- Player Items Manager updated
- inventory usable item linked when applicable
- wall detection/motion options reviewed
- save/load reviewed for batteries, ammo, durability, or item state
