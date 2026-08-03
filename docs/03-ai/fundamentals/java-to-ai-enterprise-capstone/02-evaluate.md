# Phase 2 — Evaluate Before Optimising

## Goal

Replace subjective “looks good” review with repeatable evidence. Evaluation is a product contract and a release gate, not a final testing activity.

## Evaluation assets

Maintain separate, versioned datasets:

- development set for prompt iteration;
- regression/golden set for CI and release comparison;
- adversarial set for injection, leakage and unsafe behavior;
- production samples selected through approved privacy controls;
- fine-tuning train/validation/test sets with contamination checks.

Each item needs a stable ID, task, input, expected properties, allowed evidence, tenant/security labels, difficulty, provenance and reviewer notes. Write a dataset card describing collection, limitations, consent, bias and intended use.

## Evaluation pyramid

| Layer | What to measure | Example tools |
|---|---|---|
| deterministic | schemas, parsers, citations, ACLs, tool arguments | pytest, JUnit, JSON Schema |
| retrieval | recall@k, precision@k, MRR, nDCG, context relevance | custom metrics, RAGAS |
| generation | groundedness, correctness, completeness, refusal | DeepEval, RAGAS, rubric judges |
| workflow | tool selection, step limit, final state, approval | LangGraph tests, recorded traces |
| system | p50/p95/p99, throughput, errors, cost | k6/Gatling, OTel, Prometheus |
| human | domain correctness, usefulness, fairness | blinded rubric review |

LLM-as-judge is useful but not ground truth. Version the judge prompt/model, calibrate it against humans and use multiple signals for consequential decisions.

## Baseline before changes

For every prompt, model, retriever, embedding or fine-tuning change:

1. freeze the candidate artifacts;
2. run the same dataset against baseline and candidate;
3. compare quality, safety, latency and cost;
4. inspect regressions by cohort, not only averages;
5. attach the report to the PR/release;
6. reject changes that violate thresholds.

## Example release policy

```yaml
quality:
  schema_valid_rate: ">= 0.995"
  grounded_answer_rate: ">= 0.92"
  citation_precision: ">= 0.97"
  critical_regressions: 0
security:
  cross_tenant_leaks: 0
  forbidden_tool_calls: 0
operations:
  p95_latency_regression: "<= 10%"
  cost_per_success_regression: "<= 8%"
```

Thresholds are examples; derive real values from risk and business needs.

## RAG diagnosis matrix

| Symptom | Inspect first | Likely experiment |
|---|---|---|
| answer missing known fact | retrieval recall | chunking, query rewrite, hybrid search |
| wrong source cited | citation precision | context IDs, verifier, prompt contract |
| relevant chunk ranked low | ranking trace | reranker or metadata filter |
| plausible unsupported answer | groundedness | abstention threshold, context validation |
| quality differs by tenant | cohort report | ACL/filter/data-quality audit |

Fix the failing layer. Prompt changes cannot repair missing retrieval evidence.

## Scoring and interview-domain evaluation

For question generation, assess competency coverage, uniqueness, difficulty calibration, answerability and rubric quality. For answer scoring, compare against multiple expert reviewers, report agreement, measure subgroup behavior, return evidence and confidence, and route uncertain/consequential decisions to humans. The model must not be the sole employment decision-maker.

## CI/CD integration

Run fast deterministic tests on each commit, a representative golden subset on PRs and the complete suite before model/prompt/retrieval releases. Store reports as artifacts and publish a trend. Production feedback must be reviewed and sanitized before becoming evaluation or training data.

## Evaluate gate

You pass when a single command produces a reproducible comparison report; security failures are zero-tolerance; quality thresholds are explicit; the team can locate regressions by layer/cohort; and a rollback decision can be made from evidence.

## Evidence

Publish the dataset card, metric definitions, baseline/candidate report, ten analyzed failure cases, cost/latency comparison and the CI quality-gate configuration.

