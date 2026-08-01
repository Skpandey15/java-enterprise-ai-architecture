# 10. Deployment, Observability and LLMOps

LLMOps applies software delivery, evaluation, governance and operations to prompts, models, retrieval indexes and AI workflows.

## Delivery path

```mermaid
flowchart LR
  A["Code + prompt + dataset"] --> B["Tests + evals"]
  B --> C["Security + policy gates"]
  C --> D["Immutable image"]
  D --> E["Canary/shadow release"]
  E --> F["Observe quality, cost, latency"]
  F --> G["Promote or rollback"]
  classDef ci fill:#dbeafe,stroke:#2563eb,color:#172554
  classDef cd fill:#dcfce7,stroke:#16a34a,color:#052e16
  class A,B,C,D ci
  class E,F,G cd
```

Version application code, prompt templates, model configuration, evaluation dataset, embedding model and index. A model alias changing behind your application is a production change even when code is unchanged.

## Observability

Trace request, retrieval, model calls and tools with correlation IDs. Record latency, token usage, cost, selected model, prompt version, retrieval IDs, tool outcomes and evaluation signals. Redact secrets and sensitive content.

## Reliability and cost

Use deadlines, circuit breakers, bounded retries, bulkheads, rate limits, caching where semantically safe, model fallback and graceful degradation. Budget by tenant and feature. A cheaper small model can handle routing/classification while a larger model handles difficult generation.

## Deployment

Containerise the API; use non-root images, health/readiness probes, resource limits, autoscaling and secret injection. GPU hosting adds capacity planning, batching, model loading and specialised observability; use hosted APIs first unless economics or data controls justify self-hosting.

## Incident method

Identify release/model/prompt/index versions, inspect stage traces, reproduce with a saved safe input, mitigate by rollback or feature flag, then add a regression evaluation.

## Interview checks

Explain LLMOps versus MLOps, model gateways, fallbacks, evaluation gates, canary releases, prompt/version control, observability, cost controls and rollback.
