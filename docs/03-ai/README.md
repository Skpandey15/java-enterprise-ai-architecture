# Part 3 — AI

This part covers AI development from first principles to production engineering: model APIs, chatbots, embeddings, vector search, RAG, agents, MCP, evaluation, guardrails, observability, security, cost and LLMOps.

## AI fundamentals

- [Inference APIs: Zero to Production](fundamentals/inference-apis.md) — inference versus training, API contracts, generation controls, streaming, structured output, embeddings, hosted versus self-hosted serving, reliability, security, observability and cost.
- [Fine-Tuning: Fundamentals to Production](fundamentals/fine-tuning.md) — adaptation decisions, dataset engineering, SFT, Transformers/TRL, PEFT, LoRA/QLoRA, DPO, Accelerate/DeepSpeed, experiment tracking, evaluation, packaging, serving and safe rollout.

## Study material

- [AI Development: Zero to Job-Ready (0–6 Years)](ai-development-zero-to-job-ready/README.md) — twenty progressive chapters for someone who knows AI terminology but needs a clear, hands-on path into AI development.
- [AI Frameworks and Tools](ai-development-zero-to-job-ready/12-ai-frameworks-toolkit.md) — when to use native SDKs, LangChain, LangGraph, Transformers, LlamaIndex, vector databases and evaluation/tracing tools.
- [Hugging Face Transformers Hands-On](ai-development-zero-to-job-ready/13-hugging-face-transformers-hands-on.md) — tokenizers, inference, embeddings, fine-tuning, PEFT/LoRA, quantisation and deployment.
- [LangChain and LangGraph Production Lab](ai-development-zero-to-job-ready/14-langchain-langgraph-production-lab.md) — permission-aware RAG, tools, state graphs, checkpoints, approval, evaluation and tracing.
- [Complete AI/ML Engineering Track](ai-development-zero-to-job-ready/15-ml-mathematics-foundations.md) — ML mathematics, NumPy/pandas/scikit-learn, PyTorch and Transformer internals, LoRA, MLOps, serving, cloud AI, multimodal systems and security.
- [AI-Assisted Interview Platform](ai-assisted-interview-platform.md) — an end-to-end case study combining Spring Boot, FastAPI, LiteLLM, PostgreSQL, Keycloak, Kafka, React and Kubernetes.

## Recommended path

1. Learn how inference APIs connect an application to trained models.
2. Complete the AI Development chapters in order.
3. Build the Enterprise AI Interview Assistant milestone by milestone.
4. Create an evaluation dataset before optimising prompts or changing models.
5. Add permission-aware RAG, bounded tools and human approval.
6. Compare native SDK, LangChain, LangGraph and Transformers implementations.
7. Complete the dedicated fine-tuning fundamentals and production lab.
8. Learn classical ML, PyTorch, MLOps and model serving.
9. Build a multimodal, permission-aware capstone with security and rollback evidence.

## Target capability

| Experience | Expected outcome |
|---|---|
| 0–1 year | Build model API integrations and a reliable streamed chatbot |
| 1–3 years | Build evaluated RAG and secure tool-using AI applications |
| 3–6 years | Own agent architecture, framework choices, governance, LLMOps, cost and production reliability |
