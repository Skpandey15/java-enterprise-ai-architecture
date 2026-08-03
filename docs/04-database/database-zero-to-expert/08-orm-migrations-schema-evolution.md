# 8. ORMs, Migrations and Schema Evolution

ORMs improve productivity but do not replace SQL, transaction design or query-plan knowledge.

## ORM boundaries

For Java/JPA understand entity state, persistence context, lazy/eager loading, fetch joins, batching, cascades, optimistic locking and N+1. For Python/SQLAlchemy understand sessions, unit of work, relationship loading and async-session ownership.

Avoid exposing persistence entities as API contracts. Keep transaction ownership in the application service, and inspect generated SQL.

## Migration discipline

Use Flyway, Liquibase or Alembic with immutable, reviewed and ordered migrations. Production credentials should separate runtime DML from migration DDL.

## Expand and contract

```mermaid
flowchart LR
  A["Add compatible schema"] --> B["Deploy dual-compatible code"]
  B --> C["Backfill safely"]
  C --> D["Switch reads/writes"]
  D --> E["Remove old schema later"]
```

This permits rolling deployment without requiring old and new application versions to agree instantly.

## Dangerous changes

Large table rewrites, long validation locks, immediate column renames/removals, one huge backfill transaction, and adding a non-null column without a safe rollout. Measure database-version-specific behavior.

## Lab

Implement the model in JPA and SQLAlchemy. Reproduce and eliminate an N+1 query, add optimistic locking, then evolve `candidate.full_name` into structured names using expand/backfill/contract. Prove old and new versions can coexist during rollout.

## Review checklist

Rollback may mean forward repair, not a down migration. Back up first, rehearse on production-scale data, monitor locks/WAL/replication lag, throttle backfills and record completion evidence.
