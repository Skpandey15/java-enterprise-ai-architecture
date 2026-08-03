# Phase 1 — Build the Enterprise Vertical Slice

## Goal

Build the smallest end-to-end system that proves business ownership, AI boundaries and traceability. Avoid agents and framework abstraction until the plain request path works.

## Step 1: write contracts first

Create OpenAPI contracts for interview creation, question generation, assignment and submission. Define the Java-to-Python contract independently from provider payloads.

```json
{
  "request_id": "uuid",
  "tenant_id": "tenant-a",
  "interview_id": "uuid",
  "competencies": ["java", "distributed-systems"],
  "difficulty": "SENIOR",
  "count": 5,
  "policy_version": "question-policy-v3"
}
```

The response must use JSON Schema/Pydantic validation and include `model_id`, `prompt_version`, latency, token usage and safety decisions. Never expose chain-of-thought.

## Step 2: build Spring Boot business orchestration

Use Java 21, Spring Boot, PostgreSQL, Flyway, Spring Security Resource Server and an HTTP client with explicit timeout/resilience policy.

Required behavior:

- validate the authenticated tenant and role;
- persist the generation job before calling AI;
- use idempotency keys for retried commands;
- call the AI service through a versioned client;
- validate the AI response again at the business boundary;
- persist approved question fields and provenance;
- publish an outbox event in the same database transaction.

Do not let the Python service connect to Java-owned business tables.

## Step 3: build the Python AI service

Use FastAPI, Pydantic, `httpx`, provider-neutral interfaces, structured logging and OpenTelemetry.

```python
from typing import Protocol
from pydantic import BaseModel, Field

class Question(BaseModel):
    text: str = Field(min_length=20, max_length=1200)
    rubric: list[str] = Field(min_length=1, max_length=8)
    difficulty: str

class QuestionSet(BaseModel):
    questions: list[Question]

class ModelClient(Protocol):
    async def generate(self, *, messages: list[dict], schema: dict) -> dict: ...
```

Implement deadline propagation, bounded retries only for transient failures, jittered backoff, cancellation and typed error mapping. Record model and prompt versions, not secrets or raw sensitive prompts.

## Step 4: add permission-aware RAG

Pipeline:

1. authenticate the caller at the gateway;
2. propagate trusted tenant/user claims;
3. normalize and classify the query;
4. retrieve only chunks matching tenant and document ACLs;
5. rerank authorized candidates;
6. assemble bounded context with source identifiers;
7. generate a schema-valid answer with citations;
8. verify citations refer to supplied chunks;
9. abstain when evidence is insufficient.

Store chunk text, embedding version, document version, tenant ID, ACL tags, classification and deletion state. Authorization after retrieval is too late if unauthorized content already reached the model.

## Step 5: use Kafka for justified asynchronous work

Good candidates: document ingestion, embedding generation, evaluation jobs, audit export and notifications. A synchronous model call does not become reliable merely because Kafka is added.

Each consumer needs:

- stable event key and schema version;
- idempotency record or naturally idempotent write;
- bounded retry with backoff;
- dead-letter policy and replay procedure;
- correlation/trace identifiers;
- metrics for lag, retries and poison events.

## Step 6: add tools only after direct flows work

Start with read-only tools such as `get_interview_policy`. Then add consequential operations behind explicit authorization, validation and human approval. Tool inputs come from the model and are untrusted.

Use LangGraph only when durable state, branching, checkpoint/restart or approval materially simplifies the workflow. Keep domain authorization outside prompt text.

## Required tests

- Java unit, slice, integration and Testcontainers tests;
- Python unit/async/contract tests with fake model clients;
- OpenAPI/JSON Schema compatibility tests;
- idempotency and duplicate-event tests;
- malformed/oversized model-response tests;
- retrieval ACL and citation tests;
- provider timeout, 429 and partial-stream tests.

## Build gate

You pass this phase when an authenticated user can complete one traced workflow; the provider can be replaced by a fake; duplicate requests are safe; RAG cannot cross tenant boundaries; and every response can be traced to code, prompt, model, retrieval and policy versions.

## Evidence

Commit API schemas, a component/sequence diagram, local Compose file, test report, three ADRs and a five-minute vertical-slice demo.

