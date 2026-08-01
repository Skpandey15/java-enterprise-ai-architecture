# 10. Projects and Interview Preparation

## Portfolio project definition

Build an Interview Question Service with:

- FastAPI CRUD and pagination;
- Pydantic request/response validation;
- PostgreSQL, SQLAlchemy and Alembic;
- layered endpoint/service/repository design;
- JWT validation and role-based endpoints;
- unit and integration tests;
- an async external AI-client stub with timeout handling;
- structured logs, correlation IDs, metrics and health checks;
- Dockerfile and CI checks;
- README containing setup, API examples and trade-offs.

## Questions to answer confidently

1. How do mutable and immutable objects behave?
2. List, tuple, set and dictionary—when would you choose each?
3. What are iterators, generators and decorators?
4. How do exceptions and context managers improve reliability?
5. Composition versus inheritance?
6. How does FastAPI validation and dependency injection work?
7. Sync versus async endpoint, and what blocks the event loop?
8. What is the GIL and when are processes useful?
9. How do database sessions and transactions work?
10. What causes N+1 queries?
11. Unit, integration and contract tests—what does each prove?
12. How do you validate a JWT safely?
13. How would you diagnose a slow API?
14. How do Docker health checks and Kubernetes probes differ?
15. Tell me about a defect or production issue you fixed using evidence.

## Coding practice

Implement frequency counting, deduplication preserving order, grouping, pagination, an LRU cache using standard tools, file streaming, retry with bounded attempts, and a small API endpoint with validation and tests. Always discuss complexity and edge cases.

## Two-minute experience answer

> I build Python backend services using FastAPI, Pydantic and PostgreSQL. I keep HTTP, business and persistence concerns separated, manage transactions at the service boundary, and test important success and failure paths with pytest. For I/O-heavy integrations I use async only when the full dependency path is non-blocking, with timeouts and bounded concurrency. I apply JWT validation, input constraints, structured logging and health checks, then package the service in Docker and participate in CI/CD deployment and production diagnosis. I can explain the trade-offs behind those choices and demonstrate them in a working project.

Adapt this answer to work you actually performed.

## Readiness checklist

- [ ] Can write and explain idiomatic typed Python without copying.
- [ ] Can build and document a validated REST API.
- [ ] Can model relational data and diagnose a basic slow query.
- [ ] Can write unit and integration tests for failure paths.
- [ ] Can explain sync, async, threads, processes and queues.
- [ ] Can secure, containerize and observe a service.
- [ ] Can describe one project and one debugging story with evidence.

