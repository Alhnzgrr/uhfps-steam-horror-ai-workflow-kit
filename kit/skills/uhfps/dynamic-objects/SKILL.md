# UHFPS Dynamic Objects Skill

## Purpose

Use this skill for doors, drawers, levers, valves, locks, and other UHFPS dynamic
objects.

## Core Idea

Use UHFPS `DynamicObject` and its unlock/interaction extension points instead of
building a separate door or lock system.

## UHFPS Concepts

- Dynamic objects define object type and interaction type.
- Required references depend on dynamic, mouse, or animation interaction mode.
- Custom lock behavior can implement UHFPS dynamic unlock interfaces.
- Inventory selectors can be used for key or item-based unlocks.

## Rules

- Do not animate or unlock persistent objects outside the UHFPS dynamic object flow.
- Do not search for inventory or dynamic object dependencies.
- Use explicit references and UHFPS callbacks.
- Review save/load for open/closed/locked/unlocked state.

## Setup Checklist

- Dynamic Object component attached
- target, hinge, axis, limits, and curves configured
- lock/unlock script assigned when needed
- required inventory item reference assigned
- gizmo-based setup verified
- save/load behavior reviewed
