# 12. Senior Interview Scenarios and Readiness Checklist

## Scenario prompts

### Mutable HashMap key

Explain bucket selection, equality, why mutation makes lookup fail, and how immutable value keys prevent it.

### Counter under contention

Compare `synchronized`, `AtomicLong`, `LongAdder` and database/distributed counters. State consistency needs; `LongAdder` is excellent for high-contention statistics but not an atomic sequence.

### REST service with virtual threads

Enable only after dependency compatibility and load tests. Keep database/HTTP pools, timeouts, bulkheads and rate limits bounded. Measure pinning, p99, CPU, memory and downstream saturation.

### Memory growth in Kubernetes

Check pod limit versus heap sizing, native/direct memory, thread count, allocation and post-GC live set. Use JFR/NMT/heap analysis. Avoid simply increasing memory before finding retained owners.

### Duplicate Kafka event

At-least-once delivery permits duplicates. Use stable event ID, unique processed-event constraint or naturally idempotent update, atomic business effect plus deduplication, and replay tests.

### Deadlock

Capture multiple thread dumps, identify lock cycle, mitigate impact, then impose lock ordering or remove nested shared ownership. Timeouts may reduce impact but do not prove correctness.

### Slow API after deployment

Compare release diff and RED/USE metrics, trace the critical path, profile if internal CPU/allocation is implicated, canary rollback if impact is material, then create a regression test.

## 90-second positioning

“I use Core Java at three levels. At language level, I protect equality, immutability, generics and exception contracts. At runtime level, I reason about the Java Memory Model, concurrency, class loading, allocation and GC using JFR, dumps and telemetry. At architecture level, I choose modular boundaries, integration patterns and resilience controls based on quality attributes. For modern Java, I use records, sealed types, pattern matching and virtual threads where they simplify the system, but validate framework compatibility, downstream capacity and production evidence. I build testing, security and observability into the design and document material trade-offs through ADRs.”

## Readiness checklist

You are ready when you can:

- explain mechanism, trade-off and failure mode without reciting definitions;
- write correct equality, generics and concurrency code;
- interpret thread dumps, GC signals, heap retention and JFR evidence;
- compare platform threads, virtual threads and reactive designs;
- design bounded executors, caches and retries;
- explain Java 8–25 evolution and a safe LTS upgrade;
- model with records/sealed types without creating an anemic domain;
- select patterns based on forces and reject unnecessary abstraction;
- connect testing, security, observability and delivery to architecture;
- tell two production incident stories with measured outcomes.

## Final advice

For an 18-year profile, interviewers evaluate judgment, ownership and clarity. Admit uncertainty, state assumptions, quantify scale, and explain how you would verify. That is stronger than pretending every answer is absolute.