# 11. Portfolio Project and Interview Preparation

Build an **Enterprise AI Interview Assistant** that demonstrates the complete curriculum.

## Milestones

1. FastAPI chat API using a model gateway and structured outputs.
2. Streamed chatbot with durable conversation history.
3. PostgreSQL/pgvector ingestion and permission-aware RAG with citations.
4. Question generation evaluated against a versioned dataset.
5. Tool calls for policy lookup and schedule retrieval.
6. Human approval before publishing questions or scheduling.
7. MCP server exposing bounded enterprise capabilities.
8. Authentication, tenant isolation, rate limits and audit logs.
9. Docker, CI evaluation gate, Kubernetes deployment and observability.
10. Load, failure, injection and cost tests with an architecture decision record.

## Experience-based expectations

| Experience | What to demonstrate |
|---|---|
| 0–1 year | A working, tested chatbot; prompt basics; API and token handling |
| 1–3 years | Evaluated RAG, vector search, secure tools, deployment and debugging |
| 3–6 years | Architecture trade-offs, governance, LLMOps, SLOs, cost and incident ownership |

## Two-minute interview answer

“I build AI features as tested software systems around probabilistic models. I first define the business outcome and evaluation dataset. For knowledge-based answers I use permission-aware hybrid retrieval, re-ranking, grounded prompts and citations. Model and tool outputs are untrusted, so schemas, domain validation, least privilege and human approval protect consequential operations. I version code, prompts, models and indexes; CI runs deterministic tests and AI evaluations. In production I trace retrieval, model and tool stages and monitor quality, latency, tokens, cost and safety. I use agents only when adaptive planning adds measured value; known processes remain deterministic workflows.”

## Interview question bank

- LLM, token, context window and hallucination
- RAG versus fine-tuning
- embedding, chunking, vector search and hybrid retrieval
- prompt injection and data leakage
- chatbot memory and streaming
- evaluation datasets and LLM-as-judge
- tool calling, agents and workflows
- MCP architecture and security
- model gateway, routing and fallback
- latency, cost, caching and rate limits
- deployment, observability and incident handling
- responsible AI and human approval

## Readiness checklist

You are ready when you can rebuild the project without copying a tutorial, explain every trust boundary, show evaluation evidence, reproduce a failure from traces and defend your architecture choices honestly.
