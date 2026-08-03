# 2. SQL from Basic to Advanced

SQL is declarative: describe the result, then let the optimizer choose the physical work. Correctness comes before cleverness.

## Essential progression

1. `SELECT`, filtering, ordering, aliases and null semantics
2. `INSERT`, `UPDATE`, `DELETE` with safe predicates
3. Inner/outer joins and relationship cardinality
4. Aggregation, `GROUP BY`, `HAVING`
5. Subqueries, CTEs and recursive CTEs
6. Window functions
7. Set operations and upserts

```sql
SELECT candidate_id,
       score,
       dense_rank() OVER (PARTITION BY interview_id ORDER BY score DESC) AS rank
FROM submissions
WHERE state = 'REVIEWED';
```

Window functions retain row detail while calculating across a related set. They often replace fragile self-joins.

## Correctness traps

- `NULL = NULL` is not true; use `IS NULL` or `IS NOT DISTINCT FROM`.
- A filter on the right table in `WHERE` can accidentally turn a left join into an inner join.
- `NOT IN` behaves surprisingly if the subquery contains null; prefer `NOT EXISTS`.
- Pagination using large `OFFSET` values wastes work and may shift under concurrent writes; prefer keyset pagination.
- Always make ordering deterministic.

## Lab

Write reports for active assignments, completion rates, average score by definition, top candidates per interview, unanswered questions and a recursive category tree. Validate each query with boundary data, nulls and duplicates.

## Review checklist

Explain cardinality before joining; select only needed columns; verify the plan; parameterize input; add an index only after connecting it to an access pattern.
