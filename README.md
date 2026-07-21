# Java Enterprise AI Architecture

A practical, production-oriented handbook for designing enterprise systems that combine **Java, distributed systems, cloud-native platforms, and generative AI**.

This repository is maintained by **Sunil Pandey** as an original architecture portfolio and an open learning resource. It focuses on engineering decisions, trade-offs, failure modes, security, operability, and executable reference implementations—not only diagrams.

## Who this is for

- Senior Java engineers moving toward architect roles
- Full-stack and platform engineers adding AI capabilities
- Architects designing secure, observable, cloud-native AI systems
- Interview candidates who want to explain trade-offs with practical examples

## Architecture philosophy

1. Start with business goals and measurable quality attributes.
2. Prefer the simplest architecture that satisfies the current scale.
3. Make security, observability, resilience, and cost first-class concerns.
4. Treat AI output as untrusted, probabilistic data.
5. Record important decisions and the alternatives rejected.
6. Validate diagrams with code, tests, load tests, and failure experiments.

## Learning map

| Area | Topics |
|---|---|
| Java enterprise | Java 21+, Spring Boot, modular monoliths, microservices, API design |
| Data | PostgreSQL, Flyway, transactions, caching, partitioning |
| Event-driven systems | Kafka, idempotency, outbox, Saga, ordering, retries |
| Security | OAuth 2.0, OpenID Connect, Keycloak, JWT, JWKS, Zero Trust |
| Cloud native | Containers, Kubernetes, ingress, service discovery, GitOps |
| Reliability | SLOs, resilience patterns, disaster recovery, chaos testing |
| Observability | Logs, metrics, traces, OpenTelemetry, Prometheus, Grafana |
| Enterprise AI | LLM gateways, RAG, MCP, agents, guardrails, evaluation, LLMOps |

## Repository structure

```text
docs/
├── 01-fundamentals/
├── 02-java-enterprise/
├── 03-data-and-transactions/
├── 04-event-driven-architecture/
├── 05-security/
├── 06-cloud-native/
├── 07-observability-and-resilience/
├── 08-enterprise-ai/
├── 09-case-studies/
└── 10-interview-guides/
adrs/
templates/
reference-implementations/
```

## First reference architecture

The first end-to-end case study is an **AI-assisted online interview platform**, demonstrating:

- React and TypeScript frontend
- Java 21 and Spring Boot orchestration
- Python and FastAPI AI services
- PostgreSQL with Flyway migrations
- Keycloak for identity and access management
- LiteLLM as an AI gateway
- Kafka for asynchronous workflows
- Docker and Kubernetes deployment
- Jenkins/GitHub Actions with Argo CD
- OpenTelemetry, Prometheus, Grafana, and centralized logging
- MCP-based controlled tool integration

See [AI-Assisted Interview Platform](docs/09-case-studies/ai-assisted-interview-platform.md).

## How each architecture is documented

Every substantial case study should cover:

1. Business context and scope
2. Functional and non-functional requirements
3. Capacity assumptions
4. API and data model
5. High-level architecture
6. Critical request and event flows
7. Security and privacy
8. Consistency and transaction strategy
9. Scaling and resilience
10. Observability and operations
11. Deployment and delivery
12. Cost considerations
13. Trade-offs, alternatives, and ADRs
14. Validation plan

## Roadmap

- [x] Establish repository structure and architecture principles
- [x] Add the first AI-assisted interview platform case study
- [ ] Add Java modular monolith versus microservices guidance
- [ ] Add Kafka production architecture
- [ ] Add Keycloak/OIDC/JWT/JWKS security architecture
- [ ] Add Saga and transactional outbox examples
- [ ] Add Kubernetes ingress and GitOps deployment architecture
- [ ] Add observability reference architecture
- [ ] Add enterprise RAG architecture
- [ ] Add agentic AI and MCP security architecture
- [ ] Add runnable reference implementations and failure tests

## Related implementation

Practical examples will link to [Skpandey15/Java_AI_MCP](https://github.com/Skpandey15/Java_AI_MCP) where relevant. Concepts and decisions remain documented here; executable services remain in their implementation repositories.

## Contributing

Constructive corrections and contributions are welcome. Please use an issue or pull request and explain the architectural context and trade-offs.

## License

Licensed under the [MIT License](LICENSE).
