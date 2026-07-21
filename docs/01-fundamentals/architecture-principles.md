# Enterprise Architecture Principles

## 1. Requirements before technology

Begin with business capabilities, user journeys, constraints, and measurable quality attributes. Do not select Kafka, Kubernetes, a vector database, or microservices before identifying the problem they solve.

## 2. Evolutionary architecture

Design for the current scale while preserving sensible change paths. A modular monolith can be the correct starting point. Split services when independent ownership, deployment, scaling, isolation, or data boundaries justify the operational cost.

## 3. Explicit ownership

Every service, API, event, dataset, dashboard, alert, and operational runbook needs an accountable owner.

## 4. Data is a contract

Define the system of record, consistency requirements, retention, classification, residency, lineage, and deletion policy. Schema and event evolution must remain compatible with known consumers.

## 5. Failure is normal

Document timeouts, retries, backoff, circuit breakers, bulkheads, load shedding, degraded modes, recovery, and reconciliation. Retries must be bounded and safe through idempotency.

## 6. Secure by design

Authenticate identities, authorize each resource action, minimize privilege, protect secrets, encrypt data, maintain audit trails, and distrust inputs—including AI-generated output.

## 7. Observable by design

Expose structured logs, metrics, and distributed traces correlated by request, trace, user, tenant, and business identifiers where privacy permits. Alerts should connect technical symptoms to user impact and SLOs.

## 8. Automate delivery safely

Use repeatable builds, automated tests, security scans, immutable artifacts, progressive delivery, database migration controls, and fast rollback or roll-forward paths.

## 9. Cost is an architecture attribute

Estimate infrastructure, data transfer, storage, licenses, model inference, operational labor, and failure cost. Optimize only after measuring.

## 10. Decisions remain traceable

Use ADRs for consequential decisions. Record context, chosen option, consequences, alternatives, and conditions that would trigger reconsideration.

## AI-specific principles

- Treat model output as probabilistic and untrusted.
- Keep deterministic business rules outside prompts.
- Minimize sensitive data sent to models.
- Apply authorization before retrieval and tool execution.
- Use allow-listed tools, scoped credentials, sandboxing, and approval gates.
- Version prompts, models, retrieval configuration, and evaluation datasets.
- Measure quality, latency, safety, and cost continuously.
- Provide fallbacks and human review for high-impact decisions.
