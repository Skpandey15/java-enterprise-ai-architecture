# 7. Performance Tuning and Connection Pooling

Database performance is a queueing problem across application, pool, CPU, memory, storage, locks and network. Start from evidence and a latency budget.

## Diagnostic order

1. Confirm the user-visible symptom and time window.
2. Break latency into pool wait, query execution and application processing.
3. Check workload change, errors, saturation and blocking.
4. Identify expensive queries by total time and tail latency.
5. Inspect plans and statistics.
6. change one variable, load test and compare.

## Connection pools

A connection consumes database resources. A larger pool can increase contention and latency. Bound it from database capacity across all service replicas, reserve operational headroom, set acquisition timeouts and expose pool metrics.

```text
total_possible_connections =
  replicas × pool_max_per_replica + admin/migration/other_clients
```

Use PgBouncer when connection churn or fleet size justifies it; understand transaction pooling limitations.

## Common improvements

Reduce round trips, batch writes, remove N+1 access, use keyset pagination, cache stable read-heavy results with explicit invalidation, tune queries/indexes, and partition only when it solves pruning or lifecycle needs.

## Lab

Create a repeatable load test. Record throughput, p50/p95/p99, pool wait, database CPU, I/O and locks. Compare one query fix, one index fix and two pool sizes. Publish results and rollback criteria.

## Anti-patterns

Blind parameter tuning, unbounded pools, caching inconsistent authorization data, production `EXPLAIN ANALYZE` on destructive statements, and optimizing averages while p99 or error rate worsens.
