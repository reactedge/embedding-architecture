# ADR-0006: Independent Deployment & Host Non-Ownership Principle

## Status
Accepted

---

## Context

ReactEdge widgets are designed for integration into legacy and heterogeneous host systems such as:

- WordPress
- Magento
- Static sites
- Custom CMS platforms

These systems often contain complex dependency trees, build pipelines, and runtime state management.

Requiring ownership or modification of the host application increases integration friction and reduces portability.

---

## Decision

ReactEdge widgets must be embeddable without requiring:

- Modification of the host application's build process
- Installation of JavaScript dependencies inside the host
- Ownership of the host runtime state
- Direct mutation of host-managed data structures
- Tight coupling to host framework lifecycles

Widgets are deployed as independently versioned, self-contained bundles.

As long as the public contract remains stable, widgets may be deployed independently of the host application.

---

## Architectural Implications

- Behavioural isolation is mandatory.
- Contract stability enables independent deployment.
- Integration occurs strictly through declared boundaries.
- Host systems remain authoritative over their own runtime state.

---

## Consequences

### Positive

- Reduced upgrade risk
- Lower integration friction
- Safe incremental modernization
- Feature-level autonomy
- No host build pipeline ownership

### Trade-offs

- Strict contract discipline required
- No deep host state integration
- Some complex host interactions require adapter patterns
