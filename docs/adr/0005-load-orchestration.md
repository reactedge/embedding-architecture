# ADR-0007: Widget Loader & Mounting Orchestration Layer

## Status
Accepted (Current State)

---

## Context

ReactEdge widgets are mounted inside heterogeneous host systems.

To keep widget deployment independent from the host build process and runtime state,
a lightweight loader layer is used to decide which widgets are relevant for a given page context
and to orchestrate mounting.

Without formal definition, this layer risks becoming a hidden coupling point.

---

## Decision

ReactEdge defines a dedicated loader layer responsible for:

- Identifying which widget(s) are relevant for a given page context
- Triggering widget mounting when relevant
- Optionally deferring mounting until after first paint

The loader:

- Is delivered as a standalone, versioned JavaScript bundle
- Does not require modification of the host build process
- Does not assume ownership of host runtime state
- Does not mutate host-managed data structures
- Operates as an additive orchestrator, not a host framework dependency
- May be bundled with other host JavaScript assets, while remaining logically isolated

The loader is considered part of the ReactEdge platform infrastructure,
not part of the host application.

---

## Out of Scope (Current State)

The loader does not:

- Parse widget JSON contracts
- Validate contract structure
- Enforce schema or runtime validation
- Own data fetching as a platform responsibility

These concerns are handled within widgets (or by future platform enforcement) and are formalised in separate ADRs.

---

## Responsibilities of the Loader

The loader may:

- Detect page context signals (route, template markers, DOM presence)
- Decide whether a widget should mount
- Defer mounting until after first paint

The loader must not:

- Patch host frameworks
- Intercept or override host lifecycle mechanisms
- Depend on undocumented global variables
- Introduce implicit network calls
- Take ownership of host state management

---

## Architectural Role

The loader provides:

- A consistent mounting trigger across environments
- A minimal coordination layer between page context and widget execution
- A foundation for independent feature deployment while preserving host autonomy

---

## Consequences

### Positive
- Unified mounting trigger model
- Reduced integration variability
- Allows deferred mount strategies
- Supports independent widget deployment

### Trade-offs
- Another platform layer to maintain
- Boundary discipline required to prevent scope creep
- Contract validation remains a separate concern
