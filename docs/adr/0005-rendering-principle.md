# ADR-0005: Deterministic Rendering & Behavioural Contract Principle

## Status

Accepted

------------------------------------------------------------------------

## Context

ReactEdge widgets are embedded in unstable and heterogeneous host
environments.

If rendering or behavioural integration depends on:

-   Implicit timing
-   Random values
-   Undeclared global state
-   Non-versioned event payloads
-   Host DOM mutations

Then the same configuration may produce different results across
environments.

Non-deterministic behaviour creates intermittent, difficult-to-debug
failures and undermines integration stability.

------------------------------------------------------------------------

## Decision

Widget rendering and behavioural output must be deterministic.

Rendering must be a pure function of:

-   The parsed JSON contract
-   Explicit runtime configuration

The same contract version must produce the same DOM structure and
behavioural outputs.

Event-based integrations are considered part of the behavioural contract
surface and must be explicitly versioned.

Any change to payload structure requires a version increment.

------------------------------------------------------------------------

## Examples of Contract Breakage

### ❌ Example 1: Random Value During Render

``` ts
const id = Math.random().toString(36);
```

Why this breaks determinism:

-   DOM structure changes on every mount
-   Snapshot tests become unreliable
-   Host integrations depending on structure may fail

------------------------------------------------------------------------

### ❌ Example 2: Field Rename Without Versioning

#### Original Event (implicitly v1)

``` ts
window.dispatchEvent(
  new CustomEvent("reactedge:minicart:add", {
    detail: {
      sku: "ABC-123",
      qty: 2
    }
  })
);
```

#### Later Change (no version bump)

``` ts
window.dispatchEvent(
  new CustomEvent("reactedge:minicart:add", {
    detail: {
      productSku: "ABC-123",
      quantity: 2
    }
  })
);
```

Business meaning remains identical. Only field names changed:

-   `sku` → `productSku`
-   `qty` → `quantity`

Why this breaks determinism:

-   Existing listeners expecting `detail.sku` and `detail.qty` fail
-   No version signal indicates structural change
-   Behaviour differs across deployments depending on widget version

------------------------------------------------------------------------

## Correct Versioned Approach

``` ts
// Version 1
reactedge:minicart:add:v1
{
  sku: "ABC-123",
  qty: 2
}

// Version 2
reactedge:minicart:add:v2
{
  productSku: "ABC-123",
  quantity: 2
}
```

Now:

-   Structural changes are explicit
-   Integrators can migrate safely
-   Behaviour remains deterministic per version

------------------------------------------------------------------------

## Consequences

### Positive

-   Predictable behaviour across environments
-   Stable automated tests
-   Safer version upgrades
-   Reduced integration risk
-   Explicit behavioural contract boundaries

### Trade-offs

-   Requires discipline in event design
-   Version increments needed for structural changes
-   Slightly more governance overhead