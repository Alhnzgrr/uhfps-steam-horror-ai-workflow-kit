# Validate Command

## Purpose

Decide whether a feature is complete enough to move forward.

## Required Checks

- compile status
- scene/prefab setup
- UHFPS integration
- Zenject bindings
- save/load behavior when applicable
- keyboard/mouse input
- pause/menu flow
- standalone build risk
- horror design intent

## Output Shape

```text
Validation Target:

Status:
- Pass / Partial / Fail

Confirmed Complete:

Missing Or Risky:

Evidence:

Next-Step Readiness:
```

## Rules

- Do not pass a feature with unresolved must-fix issues.
- Do not pass persistent state changes without save/load evidence.
- Do not treat Editor-only success as standalone readiness.
