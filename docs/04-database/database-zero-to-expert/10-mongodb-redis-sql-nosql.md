# 10. MongoDB, Redis and SQL-vs-NoSQL Decisions

NoSQL is not one consistency model. Choose from access patterns, aggregate boundaries, correctness and operations.

## MongoDB

Model documents around data read and changed together. Embed bounded owned data; reference independently growing or shared data. Understand indexes, compound order, multikey behavior, replica sets, read/write concerns, transactions and shard keys.

A flexible schema is still a schema—validate documents and plan evolution.

## Redis

Redis is useful for caches, counters, rate limits, locks with careful semantics, streams and short-lived coordination. It is usually not the system of record for critical business facts.

Cache keys must include tenant and authorization scope where relevant. Define TTL, invalidation, stampede protection, memory policy and degradation when Redis is unavailable.

## Decision table

| Need | PostgreSQL | MongoDB | Redis |
|---|---|---|---|
| Complex transactions/joins | Strong | Limited fit | Poor |
| Document aggregate | Good with JSONB | Strong | Limited |
| Durable system of record | Strong | Strong with design | Usually no |
| Microsecond/millisecond cache | No | No | Strong |

## Lab

Model interview questions in PostgreSQL JSONB and MongoDB, implement the same access patterns, compare constraints and indexes, then add Redis caching to a read-heavy definition endpoint. Prove invalidation and fallback behavior.

## Anti-patterns

MongoDB to avoid migrations, Redis as a hidden source of truth, distributed locks without fencing, cross-tenant cache keys, and choosing technology from popularity rather than evidence.
