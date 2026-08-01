# 1. AI, ML and Generative-AI Foundations

## Mental model

- **AI** is the broad goal of machines performing tasks associated with intelligence.
- **Machine learning** learns patterns from data instead of encoding every rule.
- **Deep learning** uses multi-layer neural networks.
- **Generative AI** creates text, images, audio or code.
- An **LLM** predicts tokens from context; it is not a database or a truth engine.

Training adjusts model weights. Inference uses fixed weights to answer a request. Most application developers begin with inference APIs, not model training.

| Approach | Use when | Example |
|---|---|---|
| Rules | Logic is deterministic | eligibility calculation |
| Classical ML | Predict from structured features | fraud score |
| LLM | Language and ambiguity dominate | summarisation |
| RAG | Answer must use changing/private knowledge | policy assistant |
| Fine-tuning | Repeated behaviour/style cannot be achieved reliably by prompt | domain classifier |

## Essential vocabulary

Know token, context window, parameters, transformer, attention, embedding, temperature, hallucination, grounding, inference, prompt, completion and multimodal input. Temperature changes sampling diversity, not factual knowledge.

```mermaid
flowchart LR
  A["User input"] --> B["Tokenizer"]
  B --> C["Token IDs"]
  C --> D["Transformer"]
  D --> E["Next-token probabilities"]
  E --> F["Generated response"]
  classDef input fill:#e0f2fe,stroke:#0284c7,color:#082f49
  classDef model fill:#ede9fe,stroke:#7c3aed,color:#2e1065
  class A,B,C input
  class D,E,F model
```

## First lab

Use a hosted model playground. Change system instruction, temperature, output format and input length. Record latency, token usage and failure cases. Verify three factual answers against an authoritative source.

## Interview checks

Explain AI versus ML versus generative AI, training versus inference, why hallucination occurs, and when an LLM is the wrong solution.
