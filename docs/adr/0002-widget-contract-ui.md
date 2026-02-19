# ADR-0002: Widget Contract Strategy

## Status

Accepted

------------------------------------------------------------------------

## Context

ReactEdge widgets run inside host systems (Magento, WordPress, static
sites) that may contain legacy constraints, CSS conflicts, and
accumulated technical debt.

To remain portable, testable, and resilient to host changes, each widget
requires a consistent and predictable integration contract.

The contract must clearly separate:

-   Business data
-   Runtime configuration
-   External integrations

Without a formal contract, widgets risk drifting into implicit coupling
with the host system.

------------------------------------------------------------------------

## Decision

Each widget must define a single, explicit integration boundary composed
of:

------------------------------------------------------------------------

### 1. Custom Element Container

Example:

``` html
<usp-widget></usp-widget>
```

The custom element acts as the root mounting boundary.

The widget must not rely on DOM outside this boundary.

------------------------------------------------------------------------

### 2. Widget-Level JSON Configuration Block

Example:

``` html
<script type="application/json" data-config>
{
  "data": {},
  "runtime": {},
  "integrations": {}
}
</script>
```

This block defines:

-   `data` → Business content rendered in the UI
-   `runtime` → Execution context (platform, mode, flags)
-   `integrations` → Explicit external dependencies

This configuration represents the full public contract between host and
widget.

------------------------------------------------------------------------

### 3. Explicit Integration Dependencies

Any required integration (e.g., Cloudflare, analytics, payment
providers) must be declared in the contract.

Global integration configuration is provided separately via the runtime
layer (e.g., `#reactedge-runtime`).

Widgets must not duplicate integration logic internally.

------------------------------------------------------------------------

### 4. Theme Layer via Documented CSS Variables

Visual customisation is provided through documented CSS variables (tokens).
Presentation-related properties must not be embedded in the runtime JSON contract unless they are inherently content-driven.

Widgets must:
- Use CSS variables for design-related properties (spacing, colours, radius, transitions, control styling).
- Provide fallback values directly within var(--token, fallback) usage.
- Avoid declaring default CSS variables at the root.
- Avoid global element selectors (e.g., button {}).
- Scope structural resets within the widget namespace.
- Participate predictably in the host CSS cascade (Light DOM).

Example:
```
.reactedge-usp {
    background-color: var(--re-usp-bg, #003652);
    color: var(--re-usp-text-color, #F6F2DF);
    gap: var(--re-usp-gap, 1.25rem);
}
```

This ensures:
- Safe default rendering
- Automatic host-level overrides
- No specificity conflicts
- Clear separation between content and design
- Reduced runtime configuration surface

The JSON contract must not contain purely presentational properties such as colours or layout styling unless explicitly justified by feature requirements.

This layer remains separate from behavioural logic.

A dedicated ADR formalises theming behaviour across Light DOM and Shadow
DOM contexts.

------------------------------------------------------------------------

### 5. Fail-Fast Validation

Widgets must validate required configuration at boot and fail fast if
invalid.

Validation enforcement is formalised separately in a dedicated ADR.

------------------------------------------------------------------------

## Consequences

### Positive

-   Consistency across all widgets
-   Clear integration boundary
-   Contract-driven testing
-   Enables disciplined versioning
-   Reduces implicit coupling

### Trade-offs

-   Requires validation logic
-   Stricter integration rules
-   Less tolerance for ad-hoc host workarounds