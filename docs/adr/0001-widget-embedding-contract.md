# ADR-0001: Widget Embedding Contract Strategy

## Status
Accepted

## Context
ReactEdge widgets must be embeddable into legacy environments (WordPress, Magento, static sites) 
without requiring ownership of the host application's runtime state or build process.
  
### Architectural Constraints
Each widget must operate independently and manage its own state lifecycle.
- Must operate in isolation and not rely on host application state.
- No host build pipeline integration is required.
- Integration is limited to static script inclusion and declarative configuration.
- Widgets must cooperate with the JavaScript host and the CSS environments.

Traditional SPA or micro-frontend approaches assume platform control,
which is not available in most legacy environments.

## Decision
Widgets are distributed as self-contained, versioned IIFE JavaScript bundles.

Each bundle:
- Contains all runtime dependencies (including React and internal libraries).
- Registers a custom HTML element as its public interface.
- Executes without requiring host build integration.
- Does not expose or depend on global variables.

Integration requires only:
1. Inclusion of the versioned script file.
2. Placement of the corresponding custom element in markup.
3. Optional declarative configuration via data-* attributes or JSON script block.

No shared runtime, module federation, or host dependency alignment is required.

## Alternatives Considered
- Module federation
- Full SPA embedding
- Script injection with global variables
- Headless-only rebuild

These approaches either required build-time coupling or platform ownership.

## Consequences

### Positive
- Portable across platforms
- Zero host build integration
- Predictable deployment

### Negative
- No shared runtime between widgets
- Potential duplication of small utilities
- Requires strict isolation discipline
