# 2. Python, Data and API Prerequisites

AI application development is mostly software engineering around probabilistic models. Learn enough Python to build maintainable services: types, collections, functions, classes, exceptions, packages, virtual environments, async I/O, HTTP, JSON and testing.

```python
from pydantic import BaseModel, Field

class ChatRequest(BaseModel):
    message: str = Field(min_length=1, max_length=4000)
    conversation_id: str

class ChatResponse(BaseModel):
    answer: str
    model: str
    input_tokens: int
    output_tokens: int
```

Use environment variables or a secret manager for API keys; never commit them. Set connection/read timeouts, retry only transient failures with backoff and jitter, and propagate correlation IDs.

## Data prerequisites

Understand CSV/JSON, text cleaning, Unicode, metadata, SQL, document permissions and data lineage. For RAG, document quality and access control often matter more than model choice.

## Minimum toolkit

- Python, virtual environments and dependency locking
- FastAPI/Pydantic for typed HTTP services
- pytest for unit and integration tests
- PostgreSQL for authoritative application data
- Docker for repeatable runtime packaging
- Git and CI for reviewable delivery

## Lab

Create a FastAPI service with `POST /chat`, validation, structured error handling, health endpoint and a fake model adapter. Write tests before connecting a paid model.

## Production mistakes

Blocking the async event loop, unbounded concurrency, logging prompts with personal data, trusting model JSON without validation, and retrying non-idempotent operations blindly.

## Interview checks

Explain async versus threads, timeout versus retry, schema validation, secret management and why model output must be treated as untrusted input.
