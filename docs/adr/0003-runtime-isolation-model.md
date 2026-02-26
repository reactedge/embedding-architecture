# ADR-3: Runtime Isolation Model

**Status:** Accepted  
**Last Updated:** 2026-02-26

---

## Context

ReactEdge widgets execute inside heterogeneous host environments:

- Magento
- WordPress
- Static sites
- Custom CMS platforms

These environments may contain:

- Unpredictable CSS systems
- Multiple JavaScript frameworks
- Dynamic DOM mutations
- Shared global namespaces

Without strict runtime isolation, widgets risk:

- State collisions
- Event conflicts
- Multi-instance instability
- Behavioural drift across environments

---

## Decision

Widgets SHALL:

1. Maintain **instance-scoped state** (no shared singleton state).
2. Support **multiple instances per page**.
3. Use **idempotent mounting logic**.
4. Namespace and version event payloads.
5. Avoid global variable pollution.
6. Default to Light DOM, with optional Shadow DOM containment.

Behavioural isolation is mandatory.  
Styling isolation is contextual.

---

## Multi-Instance Principle

The runtime must not assume a single widget instance.

### Correct Pattern

```html
<storefinder-widget data-contract="/contracts/store-1.json"></storefinder-widget>
<storefinder-widget data-contract="/contracts/store-2.json"></storefinder-widget>
```

Each instance:

- Parses its own contract
- Maintains independent state
- Emits scoped, versioned events
- Does not interfere with sibling instances

### Forbidden Pattern

- Global singleton state
- One-instance-per-type assumptions
- Adapter-level restriction of instance cardinality

Adapters must expose runtime capability, not constrain it.

---

## Mounting Discipline

Mounting must be idempotent.

Re-scanning the DOM must not:

- Double-mount components
- Duplicate event listeners
- Recreate state unexpectedly

Mount logic must detect and guard against duplicate initialisation.

---

## DOM Strategy

### Default: Light DOM

Light DOM preserves:

- Natural SEO behaviour
- Layout interoperability
- Lower integration friction

### Optional: Shadow DOM

Shadow DOM may be used when:

- Strong styling containment is required
- Host CSS is unstable
- Portability across unknown systems is prioritised

Rendering mode must not affect behavioural determinism.

---

## Event Discipline

Events are part of the behavioural contract.

Rules:

- Must be namespaced (e.g., `reactedge:storefinder:select:v1`)
- Payload structure must be versioned
- No unversioned structural changes allowed

Event collisions across instances are prohibited.

---

## Architectural Principle

> Behaviour must be isolated. Styling may integrate.

---

## Consequences

### Positive

- Safe multi-widget composition
- Cross-platform behavioural consistency
- Reduced integration fragility
- Deterministic runtime behaviour

### Trade-offs

- Strict event governance required
- Loader discipline required to avoid scope creep
- Slightly higher implementation complexity