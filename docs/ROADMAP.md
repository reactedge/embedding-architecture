## Purpose

This roadmap outlines the near-term architectural direction for ReactEdge.  
Each milestone strengthens deployment independence, contract stability, and operational safety across legacy platforms.

---

## 1. Widget Contract Discipline

**Goal:** Make widget–host interaction deterministic and governable.

- Formalise the JSON configuration schema (runtime vs settings separation).
- Enforce strict validation at runtime.
- Define clear error handling for malformed configurations.
- Establish backward compatibility rules for contract evolution.

**Outcome:**  
Widgets behave predictably across platforms and versions.

---

## 2. CDN & Version Governance

**Goal:** Decouple widget releases from host deployment cycles.

- Immutable, versioned CDN bundles.
- Explicit semantic versioning rules.
- Upgrade strategy (opt-in, version pinning supported).
- Rollback model through version targeting.

**Outcome:**  
Independent deployment

---

## 3. DOM Strategy (Light DOM vs Shadow DOM)

**Goal:** Define styling isolation intentionally.

- Clear decision matrix for Light vs Shadow DOM usage.
- Theming and CSS variable boundary rules.
- Host styling compatibility guidance.

**Outcome:**  
Isolation is deliberate and documented, not incidental.

---

## 4. Multi-Instance & Event Discipline

**Goal:** Ensure widgets can coexist safely.

- Multiple widget instances per page support.
- Scoped event naming conventions.
- No global namespace pollution.

**Outcome:**  
Widgets compose without collision or hidden coupling.

---

## 5. Observability & Failure Modes

**Goal:** Make runtime behaviour visible and resilient.

- Loading and degraded-mode UX patterns.
- Timeout handling strategy.
- Minimal, production-safe logging.

**Outcome:**  
Failures degrade gracefully instead of breaking host systems.

---

## 6. Security & Boundary Integrity

**Goal:** Avoid introducing new attack surfaces.

- Explicit CORS discipline.
- Token isolation and data minimisation.
- CSP compatibility guidelines.

**Outcome:**  
Externalised execution does not weaken host security posture.

---

## 7. Dependency & Integration Rules

**Goal:** Preserve portability across platforms.

- No reliance on host-global JavaScript state.
- Clear network dependency declaration.
- CSS scope containment rules.
- Cross-platform adapter guidelines (Magento, WordPress, others).

**Outcome:**  
Portability becomes real

---

## 8. Governance & Contribution Model

**Goal:** Ensure controlled evolution.

- ADR-first change policy.
- Version impact declaration for breaking changes.
- Deprecation and migration strategy.
- Transparent roadmap publication.

**Outcome:**  
Architectural evolution remains intentional and governed.

## 9. ADRs high level 
| ADR Name                                     | Concerns Included                                                                                                                            | Notes                                                                                              |
| -------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| **ADR-001: Contract & Compatibility Model**  | JSON schema discipline, runtime vs settings separation, validation strategy, backward compatibility rules, deprecation policy                | Foundational. Defines how widgets evolve safely over time. If this is weak, everything is fragile. |
| **ADR-002: Deployment & Version Governance** | CDN distribution, immutable bundles, semver rules, version pinning, rollback strategy, cache control                                         | Core differentiator. Explains how release lifecycle is decoupled from host lifecycle.              |
| **ADR-003: Runtime Isolation Model**         | Light vs Shadow DOM policy, multi-instance safety, instance-scoped state, event namespacing, no global pollution                             | This defines how widgets coexist safely and remain composable.                                     |
| **ADR-004: Boundary & Security Discipline**  | CORS rules, token isolation, CSP compatibility, dependency rules, no host-global reliance, data minimisation                                 | Defines trust boundaries between host and widget. Prevents accidental coupling or exposure.        |
| **ADR-005: Integration & Adapter Model**     | Adapter responsibilities, non-restriction principle, cross-platform abstraction, config injection pattern, observability & failure behaviour | Where Magento assumption issues belong. Adapters must expose capability, not constrain it.         |


| New ADR (Consolidated)                     | Concerns Included                                                                                                                               | Existing ADRs Covered     | Notes                                                                                                                     |
| ------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| **ADR-A: Contract & Compatibility Model**  | JSON contract structure, schema discipline, deterministic rendering, event versioning, localisation via contract, backward compatibility policy | 0002   •  0005   •  0008  | This becomes your strongest pillar. Contract = configuration + behaviour + events + localisation. Determinism stays here. |
| **ADR-B: Deployment & Version Governance** | Independent deployment, CDN model, semantic versioning, rollback discipline, host non-ownership                                                 | 0006   •  0009   •  0010  | Merge deployment + versioning into one architectural decision. This is your differentiator.                               |
| **ADR-C: Runtime Isolation Model**         | Light vs Shadow DOM strategy, rendering boundary, loader orchestration, multi-instance safety, instance-scoped state                            | 0003   •  0007            | Multi-instance discipline belongs here. Loader must not constrain cardinality. Behavioural isolation > styling isolation. |
| **ADR-D: Boundary & Network Discipline**   | Zero-network default, integration declaration, CORS awareness, explicit external dependencies                                                   | 0004                      | Clean security story. Keeps enterprise confidence.                                                                        |
| **ADR-E: Integration & Adapter Model**     | Embedding contract, custom element interface, adapter responsibilities, non-restriction principle, cross-platform abstraction                   | 0001                      | This is where Magento assumption issue lives. Adapter must not reduce runtime capability (e.g., multi-instance).          |
