# UHFPS Inventory Skill

## Purpose

Use this skill when adding inventory items, item selectors, item usage, or item
driven puzzle logic.

## Core Idea

Use the UHFPS Inventory Database and inventory item flows. Do not create a
parallel inventory.

## UHFPS Concepts

- Inventory items live in an Inventory Database asset.
- Pickup objects typically use UHFPS interactable item components.
- Item custom data may be stored in JSON when appropriate.
- Inventory selection can be used for lock, puzzle, and item requirement flows.

## Rules

- Do not hardcode item discovery through scene search.
- Use `ItemGuid` or UHFPS item references where the framework expects them.
- Keep item requirement logic explicit and testable.
- Review save/load when items are consumed, dropped, unlocked, or spawned.

## Setup Checklist

- Inventory Database updated
- item icon configured as Sprite when needed
- item dimensions configured
- item assigned to pickup/interactable object
- GAMEMANAGER inventory reference confirmed
- item use/equip behavior connected through UHFPS flow
