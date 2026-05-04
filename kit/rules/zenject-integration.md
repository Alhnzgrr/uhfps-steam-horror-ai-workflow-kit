# Zenject Integration Rules

## Core Rule

Use Zenject for gameplay services, systems, and composition. Do not use Zenject
to blindly take ownership of UHFPS scene components or replace UHFPS lifecycle.

## Dependency Sources

Use Zenject for:

- plain C# gameplay services
- feature coordinators
- repositories or state services
- non-scene adapters
- factories when object creation is part of the feature design

Use Inspector references for:

- scene objects
- prefabs
- Transforms
- Animators
- UHFPS components placed in scenes or prefabs
- authored UHFPS assets and databases

## Required Pattern

```text
Bind service in installer
Inject service into consumer
Use service through explicit dependency
```

Do not use:

```text
Search scene
Recover missing object
Cache accidental result
```

## Forbidden

- service locator patterns
- hidden project singletons
- auto-binding arbitrary scene components at runtime
- mixing Zenject composition with scene search fallback
- replacing UHFPS manager lifecycle with Zenject lifecycle

## UHFPS Boundary

UHFPS owns its own core managers and lifecycle.

When custom systems need to talk to UHFPS:

- use documented UHFPS APIs
- use explicit Inspector references to UHFPS scene components
- wrap UHFPS calls in small adapters when the custom service needs a clean boundary

## Review Questions

- Is this a service or a scene reference?
- Is the dependency source explicit?
- Is Zenject being used to improve composition, not to hide setup?
- Is UHFPS lifecycle respected?
