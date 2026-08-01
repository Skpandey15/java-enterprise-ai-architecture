# 3. LLM APIs, Tokens and Prompt Engineering

A production model call includes model identity, messages/instructions, input, output constraints, timeout, trace metadata and usage accounting. Wrap vendor SDKs behind your own interface so business code is portable and testable.

## Prompt structure

1. State role and goal.
2. Supply trusted context and delimit it.
3. Define constraints and refusal behaviour.
4. Request a typed output schema.
5. Include examples only when they improve measured results.

```python
class Question(BaseModel):
    text: str
    difficulty: str
    expected_points: list[str]

async def generate(topic: str) -> Question:
    result = await gateway.generate(
        system="Create one interview question. Do not invent citations.",
        user=f"Topic: {topic}",
        response_model=Question,
        timeout_seconds=20,
    )
    return result
```

Structured output reduces parsing ambiguity but does not guarantee semantic correctness. Validate ranges, identifiers, permissions and business rules after parsing.

## Tokens, latency and cost

Approximate cost is input tokens × input price plus output tokens × output price. Total latency includes queueing, retrieval, model time, tool calls and network time. Stream output to improve perceived latency, but still support cancellation and final usage recording.

## Prompt injection

Data retrieved from documents or users is not trusted instruction. Separate instructions from content, allow-list tools, enforce authorization outside the model and never let a prompt grant access.

## Lab

Create five prompt versions for question generation. Build a 30-case dataset and compare format validity, relevance, hallucination rate, latency and cost. Select with evidence.

## Interview checks

Explain system versus user instructions, temperature, structured output, context limits, prompt injection and why prompt engineering requires evaluation.
