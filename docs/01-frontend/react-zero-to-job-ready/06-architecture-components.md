# 6. Architecture and Reusable Components

Organize by business feature, keeping components, hooks, API adapters, schemas and tests close.

```text
src/
  app/             routing, providers, global boundaries
  features/
    interview/     UI, hooks, API, schemas, tests
    results/
  shared/
    ui/            accessible primitives
    api/           HTTP client
    utils/
```

## Component layers

```mermaid
flowchart TD
  A["Routes and pages"] --> B["Feature components"]
  B --> C["Shared UI primitives"]
  B --> D["Feature hooks"]
  D --> E["API adapters"]
  classDef page fill:#ede9fe,stroke:#7c3aed,color:#2e1065
  classDef feature fill:#dcfce7,stroke:#16a34a,color:#052e16
  classDef boundary fill:#fee2e2,stroke:#dc2626,color:#450a0a
  class A page
  class B,C,D feature
  class E boundary
```

Use design tokens and accessible primitives rather than copy-pasted styled controls. Keep transport DTOs at the API boundary; map them into UI-friendly models. Error boundaries handle unexpected render failures, while ordinary request errors belong in the feature UI.

Lazy-load meaningful route bundles. Avoid premature micro-frontends: they add independent deployment benefits only when team and domain boundaries justify runtime complexity.

## Interview checks

Discuss feature folders, container/presentation separation, composition, design systems, error boundaries and micro-frontend trade-offs.
