# ADR-0003: Rendering Boundary Strategy (Light DOM vs Shadow DOM)

## Status
Accepted

---

## Context

ReactEdge widgets are embedded into legacy platforms with existing CSS systems.

Shadow DOM provides strong styling isolation but can introduce:

- Integration friction
- SEO and content visibility considerations
- Stacking and layout constraints
- Reduced DOM interoperability

Light DOM allows natural integration with the host environment but introduces styling coupling.

ReactEdge’s primary objective is to reduce **behavioural and structural coupling**, not to eliminate styling interaction entirely.

---

## Decision

ReactEdge supports both Light DOM and Shadow DOM rendering modes.

### Default Mode: Light DOM

Light DOM is the preferred default because it:

- Preserves natural SEO behaviour
- Allows normal stacking and layout interaction
- Enables reuse of existing host styling
- Minimises integration friction

Styling coupling in Light DOM is **accepted as an architectural trade-off**.

CSS is considered a low-cost, adjustable layer compared to JavaScript or data coupling.

---

### Optional Mode: Shadow DOM

Shadow DOM may be used when:

- Strong styling isolation is required
- Portability across unknown host environments is prioritised
- Host CSS is too unpredictable for stable rendering

Shadow DOM is treated as a containment strategy, not the default model.

---

## In All Modes

Regardless of rendering strategy:

- JavaScript, state management, and rendering logic remain isolated.
- JavaScript dependencies are decoupled from the host system.
- Data rendering is contract-driven and deterministic.
- No implicit behavioural coupling with host systems is permitted.

---

## Consequences

### Light DOM

**Positive**
- Full SEO compatibility
- Natural DOM interoperability
- Reduced integration friction

**Trade-offs**
- Styling boundaries are softer
- CSS leakage must be managed consciously

---

### Shadow DOM

**Positive**
- Strong styling isolation
- High portability

**Trade-offs**
- Reduced DOM interoperability
- Potential SEO and integration constraints

---

## Architectural Position

ReactEdge prioritises behavioural isolation over styling isolation.

Styling may couple.  
Behaviour does not.
