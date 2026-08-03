# 6. Transactions, Isolation and Locking

ACID means atomicity, consistency, isolation and durability. Isolation controls which concurrent histories are allowed; it does not remove the need to model invariants.

## PostgreSQL isolation

| Level | Typical behavior |
|---|---|
| Read committed | Each statement sees a fresh committed snapshot |
| Repeatable read | Transaction sees a stable snapshot; serialization anomalies are restricted |
| Serializable | Detects unsafe dependency patterns; callers must retry |

Use the lowest level that preserves the business invariant, then test it under concurrency.

## Concurrency tools

- Row locks with `SELECT ... FOR UPDATE`
- Atomic conditional updates
- Unique and exclusion constraints
- Optimistic version columns
- Advisory locks for carefully scoped coordination
- Serializable transactions with bounded retry

```sql
UPDATE assignment
SET state = 'STARTED', version = version + 1
WHERE id = :id AND state = 'SCHEDULED' AND version = :expected_version;
```

Zero updated rows means conflict or invalid state; it is not success.

## Deadlocks

Deadlocks arise from inconsistent lock order. PostgreSQL aborts one participant. Keep transactions short, access objects consistently, avoid user/network waits inside a transaction, and retry only safe idempotent units.

## Lab

Simulate lost updates, write skew, lock waits and a deadlock with concurrent sessions. Protect a “one active submission per assignment” invariant using two different techniques and explain the trade-off.

## Production evidence

Monitor transaction age, blocked sessions, lock graphs, deadlocks, retry rate and long-running transactions. Never “fix” contention by blindly increasing timeouts.
