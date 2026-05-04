# UHFPS Core Protection Rules

## Core Rule

UHFPS is the base framework. Do not rewrite or replace its core systems.

## Protected Systems

Do not rewrite or replace:

- Player Controller
- HEROPLAYER setup
- GAMEMANAGER setup
- Inventory System
- Save/Load System
- Interaction System
- Objectives System
- Game Manager
- Input System
- Dynamic Objects
- Jumpscare System
- Cutscene System
- Motion Controller
- Player Items framework

## Allowed Extension Paths

Prefer:

- documented UHFPS APIs
- custom MonoBehaviours that implement UHFPS interfaces
- UHFPS component events
- explicit Inspector assignments
- adapters around UHFPS APIs
- Zenject services for project-specific logic outside UHFPS core

## Singleton/API Policy

UHFPS documentation uses some singleton-style official APIs such as managers and
runtime access points.

These may be used only when:

- the access is documented or already established in the project
- the code is integrating with UHFPS rather than bypassing it
- custom logic remains outside protected UHFPS core files
- setup requirements are still explicit

Do not invent new singleton access patterns.

## Parallel System Rule

Do not build a new inventory, objective, interaction, save/load, or player item
system when UHFPS already provides that feature.

Extend or adapt the UHFPS system instead.

## Review Questions

- Is this modifying UHFPS core unnecessarily?
- Is this duplicating an existing UHFPS feature?
- Is the extension using the documented UHFPS flow?
- Could this break future UHFPS updates?
