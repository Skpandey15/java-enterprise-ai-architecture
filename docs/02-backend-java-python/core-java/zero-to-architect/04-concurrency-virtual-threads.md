# 4. Concurrency, Java Memory Model and Virtual Threads

## Correctness first

Concurrency failures include lost updates, stale reads, unsafe publication, deadlock, livelock, starvation, priority inversion and resource exhaustion. Define invariants and ownership before choosing a lock or executor.

`volatile` gives visibility and ordering for a variable, not atomicity for `count++`. `synchronized` provides mutual exclusion plus happens-before. Atomics are strong for single-variable lock-free state. `Lock` supports timed/interruptible acquisition and multiple conditions.

Prefer higher-level utilities: immutable state, `ConcurrentHashMap`, blocking queues, semaphores, latches, structured task coordination, and bounded executors.

## Executor design

Separate workloads when one can starve another. Bound queues and define rejection/backpressure. Unbounded queues hide overload as latency and memory growth. Little's Law connects concurrency, throughput and latency: (L = lambda W).

## Virtual threads

Virtual threads are lightweight JVM-managed threads well suited to high-concurrency, blocking I/O. They improve scalability, not the speed of a database or downstream service.

```java
try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
    var future = executor.submit(() -> client.fetch());
    return future.get();
}
```

Production rules:

- Keep connection pools, rate limits and bulkheads bounded.
- Do not pool virtual threads as a scarce resource.
- Measure pinning/blocking behavior, CPU and downstream saturation.
- ThreadLocal-heavy designs can become expensive at very high concurrency.
- CPU-bound work remains limited by cores; use controlled parallelism.
- Kafka consumer concurrency remains partition-bound and framework-controlled.

## Platform vs virtual vs reactive

| Model | Best fit | Cost |
|---|---|---|
| Platform-thread pool | Controlled concurrency, CPU or legacy | Larger per-thread footprint |
| Virtual thread per task | Blocking request/response I/O | Must control downstream resources |
| Reactive | End-to-end streaming/backpressure | Higher cognitive/debugging complexity |

Do not combine models without a reason. Virtual threads can simplify imperative services; reactive remains valuable for streaming and when libraries are natively non-blocking.

## Review checklist

- Which state is shared and who owns it?
- What establishes happens-before?
- Are compound actions atomic?
- Is concurrency bounded at every external dependency?
- Can cancellation and timeouts propagate?
- Are tasks idempotent?
- What do thread dumps and metrics show during saturation?

A senior answer includes load shedding and bulkheads, not only locks.