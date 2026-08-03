# Java Enterprise AI Architecture

A practical, production-oriented handbook for designing enterprise systems that combine **Java, distributed systems, cloud-native platforms, and generative AI**.

This repository is maintained by **Sunil Pandey** as an original architecture portfolio and an open learning resource. It focuses on engineering decisions, trade-offs, failure modes, security, operability, and executable reference implementations—not only diagrams.

## Five-part learning map

| Part | Focus |
|---|---|
| [1. Frontend](docs/01-frontend/) | React, TypeScript, browser security, testing, performance, accessibility, and micro-frontends |
| [2. Backend — Core Java and Python](docs/02-backend-java-python/) | Core Java Zero to Architect, Spring Boot, FastAPI, Kafka, resilience, security and runtime engineering |
| [3. AI](docs/03-ai/) | AI Development Zero to Job-Ready, inference APIs, LLM gateways, RAG, agents, MCP, guardrails, evaluation, observability and LLMOps |
| [4. Database](docs/04-database/) | Vendor-neutral SQL and JOINs, PostgreSQL, MongoDB, modelling, transactions, consistency, outbox, partitioning and recovery |
| [5. CI/CD](docs/05-ci-cd/) | Build pipelines, quality gates, artifacts, Docker, Kubernetes, Jenkins, Argo CD, GitOps, and Ingress |

## Featured learning guides

- [Python for AI Development: Minimum Practical Tutorial](docs/03-ai/fundamentals/python-for-ai-development.md) — the focused Python bridge needed for model APIs, data/evaluation work, FastAPI, async I/O, testing and a first typed AI service
- [Java Developer/Architect to AI Migration Guide](docs/03-ai/fundamentals/java-developer-architect-to-ai-migration-guide.md) — preserve enterprise Java strengths while adding Python, ML/LLM foundations, RAG, agents, evaluation, security, LLMOps and AI-architecture decision skills
- [Database: Zero to Expert](docs/04-database/database-zero-to-expert/README.md) — thirteen progressive chapters covering vendor-neutral SQL, JOINs, modelling, PostgreSQL, indexing, transactions, performance, ORMs, migrations, scale, NoSQL, security, recovery and distributed-data architecture
- [SQL JOINs: Zero to Expert](docs/04-database/database-zero-to-expert/13-sql-joins-zero-to-expert.md) — join types, cardinality, ON vs WHERE, semi/anti joins, algorithms, indexing, execution plans and ORM behavior
- [AI Development: Zero to Job-Ready (0–6 Years)](docs/03-ai/ai-development-zero-to-job-ready/README.md) — twenty progressive chapters covering ML foundations, classical ML, deep learning, LLM APIs, RAG, agents, frameworks, fine-tuning, MLOps, multimodal AI, cloud and security
- [Inference APIs: Zero to Production](docs/03-ai/fundamentals/inference-apis.md) — model inference, API contracts, streaming, structured output, embeddings, reliability, security, observability, cost and serving trade-offs
- [Fine-Tuning: Fundamentals to Production](docs/03-ai/fundamentals/fine-tuning.md) — SFT, datasets, chat templates, Transformers, TRL, PEFT, LoRA/QLoRA, DPO, MLflow, distributed training, evaluation, serving, canaries and rollback
- [Complete AI/ML Engineering Track](docs/03-ai/ai-development-zero-to-job-ready/15-ml-mathematics-foundations.md) — mathematics, scikit-learn, PyTorch, Transformer internals, LoRA, MLOps, model serving, cloud AI, multimodal systems and security
- [AI Frameworks: LangChain, LangGraph, Transformers and More](docs/03-ai/ai-development-zero-to-job-ready/12-ai-frameworks-toolkit.md) — framework choices, LlamaIndex, vector stores, RAGAS, DeepEval, LangSmith and OpenTelemetry
- [Hugging Face Transformers Hands-On](docs/03-ai/ai-development-zero-to-job-ready/13-hugging-face-transformers-hands-on.md)
- [LangChain and LangGraph Production Lab](docs/03-ai/ai-development-zero-to-job-ready/14-langchain-langgraph-production-lab.md)
- [ReactJS: Zero to Job-Ready (0–3 Years)](docs/01-frontend/react-zero-to-job-ready/README.md) — ten progressive chapters covering TypeScript, React, hooks, routing, forms, APIs, state, testing, security, accessibility, performance, deployment and interview preparation
- [Python Backend: Zero to Job-Ready (0–3 Years)](docs/02-backend-java-python/python/zero-to-job-ready/README.md) — ten progressive chapters covering Core Python, FastAPI, SQLAlchemy, async I/O, pytest, security, observability, deployment and interview preparation
- [Core Java: Zero to Architect](docs/02-backend-java-python/core-java/) — twelve senior-level chapters covering language design, collections, JVM/GC, concurrency and virtual threads, I/O, functional Java, reflection/modules, Java 8–25 evolution, testing, security, production debugging, design patterns and interview scenarios
- [Enterprise Architecture Principles](docs/02-backend-java-python/core-java/fundamentals/architecture-principles.md)
- [Java Modular Monolith vs Microservices](docs/02-backend-java-python/core-java/modular-monolith-vs-microservices.md)
- [Kafka Fundamentals: Zero to Hero](docs/02-backend-java-python/core-java/event-driven/kafka-fundamentals-zero-to-hero.md)
- [Kafka Production Architecture](docs/02-backend-java-python/core-java/event-driven/kafka-production-architecture.md)
- [Saga and Transactional Outbox](docs/04-database/saga-and-transactional-outbox.md)
- [Keycloak, OAuth 2.0, OIDC, JWT and JWKS](docs/02-backend-java-python/core-java/security/keycloak-oauth2-oidc-jwt-jwks-security-architecture.md)
- [Complete CI/CD Pipeline](docs/05-ci-cd/complete-ci-cd-pipeline.md)
- [Docker and Kubernetes Fundamentals](docs/05-ci-cd/docker-kubernetes-fundamentals.md)
- [Jenkins CI/CD Pipeline](docs/05-ci-cd/jenkins-ci-cd-pipeline.md)
- [AI-Assisted Interview Platform](docs/03-ai/ai-assisted-interview-platform.md)

## Architecture philosophy

1. Start with business goals and measurable quality attributes.
2. Prefer the simplest architecture that satisfies the current scale.
3. Make security, observability, resilience, and cost first-class concerns.
4. Treat AI output as untrusted, probabilistic data.
5. Record important decisions and rejected alternatives.
6. Validate diagrams with code, tests, load tests, and failure experiments.

## Roadmap

- [x] Establish five-part handbook structure
- [x] Add Core Java Zero-to-Architect curriculum for senior engineers
- [x] Add Python Zero-to-Job-Ready curriculum for 0–3 years
- [x] Add ReactJS Zero-to-Job-Ready curriculum for 0–3 years
- [x] Add AI Development Zero-to-Job-Ready curriculum for 0–6 years
- [x] Add Java developer/architect to AI migration guide
- [x] Add minimum practical Python tutorial for AI development
- [x] Add AI inference API fundamentals
- [x] Add extensive fine-tuning fundamentals and production lab
- [x] Add hands-on LangChain, LangGraph and Hugging Face Transformers track
- [x] Complete broader AI/ML engineering track: classical ML, deep learning, fine-tuning, MLOps, serving, cloud, multimodal AI and security
- [x] Add Database Zero-to-Expert curriculum from SQL foundations to distributed-data architecture
- [x] Add Java, Kafka, security, database and CI/CD architecture guides
- [ ] Add observability reference architecture
- [ ] Add runnable reference implementations and failure tests

## Related implementation

Practical examples link to [Skpandey15/Java_AI_MCP](https://github.com/Skpandey15/Java_AI_MCP) where relevant. Concepts remain documented here; executable services remain in implementation repositories.

## Contributing

Constructive corrections and contributions are welcome. Explain the architectural context, evidence and trade-offs.

## License

Licensed under the [MIT License](LICENSE).
