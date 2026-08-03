# 5. Indexes and Query Execution Plans

An index is an access path. It helps only when its shape matches predicates, joins, ordering and selectivity.

## Index families

| Type | Good for |
|---|---|
| B-tree | Equality, range and ordered access |
| Hash | Equality only; B-tree is usually more flexible |
| GIN | Arrays, JSONB containment and full-text |
| GiST | Ranges, geometry and specialized operators |
| BRIN | Very large physically correlated tables |

Composite index column order matters. Equality columns commonly precede ranges; ordering and covering needs may change the design. Partial indexes target a stable subset; expression indexes match computed predicates.

```sql
CREATE INDEX idx_assignment_active_candidate
ON assignment (candidate_id, starts_at DESC)
WHERE state IN ('SCHEDULED','STARTED');
```

## Read plans

Use `EXPLAIN (ANALYZE, BUFFERS)` safely on representative queries. Compare estimated versus actual rows, scan type, loops, sort/hash spills, buffer hits/reads and total time. A sequential scan is correct for a large fraction of a table.

## Failure patterns

- Functions or casts prevent an indexable predicate.
- Stale/skewed statistics cause cardinality errors.
- An index removes reads but increases write amplification.
- N+1 queries hide above individually fast plans.
- Adding every suggested index bloats storage and vacuum work.

## Lab

Generate at least one million assignments. Diagnose a slow dashboard query, create two candidate indexes, compare plans and write cost, retain the evidence-based winner, then remove an unused index.

**Expert principle:** tune the whole workload, not a single query in isolation.
