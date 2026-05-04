# Runtime Reference Safety Rules

## Core Rule

Missing references are setup errors.

Do not repair missing setup at runtime. Fail fast with a clear error that tells
the developer exactly what must be assigned and where.

## Forbidden

- `GameObject.Find`
- `FindObjectOfType`
- `FindAnyObjectByType`
- scene search logic
- hierarchy search as fallback dependency resolution
- `Camera.main` as fallback
- service locators
- hidden singletons created by project code
- auto-binding missing dependencies in `Awake`, `Start`, or `OnEnable`

## GetComponent Policy

`GetComponent` is forbidden when used to resolve a missing dependency.

Allowed narrow case:

- the component is on the same GameObject
- the script has `[RequireComponent(typeof(...))]`
- the component is cached once as an owned local dependency
- failure is treated as a setup or prefab authoring error

Do not use `GetComponentInChildren` or `GetComponentInParent` as fallback
dependency recovery.

## Inspector References

Use `[SerializeField] private` for required Unity object references such as:

- GameObject
- Transform
- Component
- Animator
- Collider
- AudioSource
- UHFPS scene components
- prefab and asset references

Validate required references explicitly and throw descriptive exceptions.

## Error Standard

Bad:

```csharp
Debug.Log("Missing reference");
```

Good:

```csharp
throw new InvalidOperationException(
    "GeneratorPuzzle: FuseSocket reference is missing. Assign it on the GeneratorPuzzle component in the Inspector.");
```

## Review Questions

- Is any missing dependency being searched for at runtime?
- Does the error explain the object, component, and fix?
- Are scene and prefab references explicit?
- Is local `GetComponent` guarded by `[RequireComponent]` and used only for owned components?
