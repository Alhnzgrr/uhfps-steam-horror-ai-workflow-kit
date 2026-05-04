# UHFPS Game Manager Skill

## Purpose

Use this skill when integrating with GAMEMANAGER, custom UI references, manager
modules, or global UHFPS runtime services.

## Core Idea

Respect UHFPS Game Manager ownership. Use documented access points and explicit
setup without creating a competing global manager.

## UHFPS Concepts

- GAMEMANAGER owns UHFPS-level systems and references.
- Custom UI references can be registered in UHFPS Game Manager.
- Manager modules can be ScriptableObject-based modules for manager behavior.
- Documented UHFPS singleton-style APIs may be valid integration points.

## Rules

- Do not replace GAMEMANAGER.
- Do not add unrelated responsibilities to a custom global manager.
- Do not use Game Manager access as a general service locator for project code.
- Wrap UHFPS access in small adapters when custom services need testable boundaries.

## Setup Checklist

- required custom UI references added
- manager module need justified
- module has no scene references if ScriptableObject-based
- Zenject service boundary defined when custom systems depend on Game Manager data
