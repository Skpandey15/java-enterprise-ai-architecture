# 8. Modern Java Evolution: Java 8 Through 25

Focus on features that change design and operations, not a memorized release list.

| Era | Important capabilities | Architectural effect |
|---|---|---|
| Java 8 | Lambdas, streams, Optional, CompletableFuture, java.time | Functional APIs and modern async composition |
| 9–11 | JPMS, collection factories, HTTP client, flight recorder availability | Modules, better platform APIs, diagnostics |
| 12–17 | Switch expressions, text blocks, records, pattern matching, sealed classes | Concise immutable models and closed hierarchies |
| 18–21 | UTF-8 default, virtual threads, record patterns, pattern switch, sequenced collections | Simpler high-concurrency blocking services |
| 22–25 | Continued language/runtime evolution, including finalized and preview features by release | Evaluate support status before production adoption |

Java 17, 21 and 25 are LTS releases, but support policy depends on the chosen vendor/distribution. Verify framework, agents, drivers, build tools and container images before upgrading.

## Upgrade strategy

1. Inventory runtime, libraries, bytecode agents, reflection and unsupported APIs.
2. Upgrade build/test toolchains before production runtime.
3. Compile with warnings and run static analysis.
4. Execute unit, integration, contract, performance and soak tests.
5. Compare GC, startup, CPU, memory and latency.
6. Use canary rollout and a rollback-compatible data strategy.
7. Record removed JVM flags and behavior changes.

Use `jdeps`, `jdeprscan`, dependency scanning, JFR and production-like benchmarks. Avoid `--add-opens` as a permanent substitute for fixing illegal access.

## Preview features

Preview features require explicit compiler/runtime flags and may change. They are suitable for evaluation, not casual production dependencies. Distinguish preview language/API features from incubator modules.

## Modern modelling example

Records plus sealed interfaces and exhaustive pattern switches can model closed domain outcomes clearly. Virtual threads can simplify request-per-thread I/O services. Neither removes the need for domain boundaries, resource limits or compatibility governance.

## Interview answer

Explain one actual migration: baseline, incompatibilities, test evidence, performance delta, rollout and rollback. “We upgraded because it was LTS” is incomplete; business risk and operational evidence matter.