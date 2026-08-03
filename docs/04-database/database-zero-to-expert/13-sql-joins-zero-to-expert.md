# 13. SQL JOINs: Zero to Expert

A join combines rows using a relationship. The syntax is simple; expert use requires reasoning about cardinality, nulls, duplicates, selectivity, execution algorithms and indexes.

Examples use vendor-neutral SQL where possible. PostgreSQL-specific plan commands are labelled explicitly.

## Learning outcomes

After this chapter, you should be able to:

- choose the correct join type from the business question;
- predict output cardinality before running a query;
- prevent accidental row multiplication and outer-join bugs;
- express semi-joins and anti-joins safely;
- choose among joins, subqueries, CTEs and window functions;
- read nested-loop, hash-join and merge-join plans;
- design indexes that support joins without over-indexing;
- diagnose slow or incorrect multi-table queries.

## Example model

```text
candidate 1 ──< assignment >── 1 interview_definition
assignment 1 ── 0..1 submission 1 ──< answer
interview_definition 1 ──< question
```

The examples use singular table names consistently: `candidate`, `assignment`, `interview_definition`, `submission`, `question` and `answer`.

## 1. INNER JOIN

An inner join returns only matching rows.

```sql
SELECT a.id AS assignment_id,
       c.full_name,
       i.title
FROM assignment AS a
JOIN candidate AS c
  ON c.id = a.candidate_id
JOIN interview_definition AS i
  ON i.id = a.interview_id;
```

Use it when the result is meaningful only if both sides exist. Foreign keys may guarantee a match, but optional relationships and filters can still remove rows.

## 2. LEFT OUTER JOIN

A left join preserves every row from the left side and fills unmatched right-side columns with `NULL`.

```sql
SELECT a.id,
       c.full_name,
       s.submitted_at
FROM assignment AS a
JOIN candidate AS c
  ON c.id = a.candidate_id
LEFT JOIN submission AS s
  ON s.assignment_id = a.id;
```

This answers: “Show every assignment, including those without a submission.”

### The ON-vs-WHERE trap

This query unintentionally removes assignments without reviewed submissions:

```sql
SELECT a.id, s.score
FROM assignment AS a
LEFT JOIN submission AS s
  ON s.assignment_id = a.id
WHERE s.state = 'REVIEWED';
```

Putting the right-side filter in `WHERE` rejects the null-extended rows, making the result behave like an inner join. Preserve assignments by moving the condition into `ON`:

```sql
SELECT a.id, s.score
FROM assignment AS a
LEFT JOIN submission AS s
  ON s.assignment_id = a.id
 AND s.state = 'REVIEWED';
```

These queries answer different business questions. Treat predicate placement as semantics, not formatting.

## 3. RIGHT and FULL OUTER JOIN

A right join preserves the right side. It can usually be rewritten as a left join by swapping table order, which many teams find easier to read.

A full outer join preserves unmatched rows from both sides:

```sql
SELECT c.id AS candidate_id,
       a.id AS assignment_id
FROM candidate AS c
FULL OUTER JOIN assignment AS a
  ON a.candidate_id = c.id;
```

Use full joins mainly for reconciliation, migration validation and comparing two datasets. They are less common in transactional request paths.

## 4. CROSS JOIN

A cross join creates the Cartesian product: every left row paired with every right row.

```sql
SELECT c.id, i.id
FROM candidate AS c
CROSS JOIN interview_definition AS i
WHERE i.state = 'PUBLISHED';
```

If there are 10,000 candidates and 100 interviews, the intermediate result can contain one million rows. Use cross joins deliberately for combinations, calendars, test data or matrices; an accidentally missing join predicate is a production hazard.

## 5. SELF JOIN

A self join relates rows within one table. For example, an employee hierarchy:

```sql
SELECT e.full_name AS employee,
       m.full_name AS manager
FROM employee AS e
LEFT JOIN employee AS m
  ON m.id = e.manager_id;
```

Use clear aliases because each alias represents a distinct role. For arbitrary-depth trees, a recursive CTE is usually more appropriate than many repeated self joins.

## 6. Many-to-many and multi-table joins

A junction table resolves a many-to-many relationship:

```sql
SELECT i.title,
       t.name AS topic_name
FROM interview_definition AS i
JOIN interview_topic AS it
  ON it.interview_id = i.id
JOIN topic AS t
  ON t.id = it.topic_id;
```

Declare a composite unique key such as `UNIQUE (interview_id, topic_id)` to prevent duplicate relationships.

For longer join chains, verify one relationship at a time. A correct five-table query is built from five understood cardinalities, not from trial and error.

## 7. Cardinality and duplicate rows

Before running a join, state whether each relationship is one-to-one, one-to-many or many-to-many.

If one assignment has five questions and one submission has five answers, joining both child collections only by assignment can produce 25 rows. That is not a database duplicate; it is row multiplication caused by the join graph.

Strategies include:

- join children through their complete relationship keys;
- aggregate each child set before joining;
- use `EXISTS` when only presence matters;
- calculate independent metrics in separate CTEs;
- use `DISTINCT` only when duplicate elimination is truly the required semantics.

Do not use `DISTINCT` as a bandage for an unexplained cardinality error.

## 8. Semi-join with EXISTS

A semi-join returns left-side rows for which a match exists, without copying right-side columns or multiplying rows.

```sql
SELECT c.id, c.full_name
FROM candidate AS c
WHERE EXISTS (
  SELECT 1
  FROM assignment AS a
  WHERE a.candidate_id = c.id
    AND a.state = 'STARTED'
);
```

Prefer `EXISTS` over an inner join plus `DISTINCT` when the requirement is simply “candidates who have at least one active assignment.”

## 9. Anti-join with NOT EXISTS

An anti-join returns left-side rows for which no match exists:

```sql
SELECT a.id
FROM assignment AS a
WHERE NOT EXISTS (
  SELECT 1
  FROM submission AS s
  WHERE s.assignment_id = a.id
);
```

Prefer `NOT EXISTS` to `NOT IN (subquery)` because a null returned by `NOT IN` can make the result unknown and unexpectedly return no rows.

A left join with `WHERE right_key IS NULL` can also express an anti-join, but `NOT EXISTS` usually communicates intent more directly.

## 10. Non-equi and range joins

Join predicates are not limited to equality. This example maps a score to a grade band:

```sql
SELECT s.id, s.score, g.grade
FROM submission AS s
JOIN grade_band AS g
  ON s.score >= g.minimum_score
 AND s.score <  g.maximum_score;
```

Ensure ranges do not overlap unless multiple matches are intentional. Range joins can be expensive; model constraints and specialized indexes may help depending on the database.

## 11. Joining aggregated data

Aggregate before joining when each side has a different grain:

```sql
WITH answer_totals AS (
  SELECT submission_id, COUNT(*) AS answer_count
  FROM answer
  GROUP BY submission_id
)
SELECT s.id,
       s.score,
       COALESCE(at.answer_count, 0) AS answer_count
FROM submission AS s
LEFT JOIN answer_totals AS at
  ON at.submission_id = s.id;
```

This makes the grain explicit: one output row per submission.

## 12. JOIN vs subquery vs CTE vs window function

| Need | Strong starting choice |
|---|---|
| Return columns from related rows | JOIN |
| Test whether a related row exists | EXISTS / NOT EXISTS |
| Isolate a transformation or pre-aggregate | CTE or derived table |
| Compare rows while retaining their detail | Window function |
| Reuse a scalar lookup | Scalar subquery, after cardinality is guaranteed |

Modern optimizers may transform equivalent forms into similar plans. Choose the form that expresses intent clearly, then verify the actual plan and workload behavior.

## 13. NULL semantics

`NULL` means unknown or absent; it is not equal to another null. In ordinary equality joins, two null keys do not match.

Use null-safe equality only when the business meaning truly treats nulls as equal. PostgreSQL supports `IS NOT DISTINCT FROM`; other databases have different syntax. Prefer non-null relationship keys and explicit data modelling over clever null matching.

## 14. Physical join algorithms

The optimizer chooses a physical algorithm; SQL does not normally prescribe one.

| Algorithm | Typically effective when | Common risk |
|---|---|---|
| Nested-loop join | Outer input is small and inner lookup is indexed | Repeated scans when estimates are wrong |
| Hash join | Equality join over medium/large unsorted inputs | Memory pressure and disk spill |
| Merge join | Inputs are already sorted or sortable on join keys | Sort cost and sensitivity to data volume |

A nested loop is not inherently bad, and a hash join is not inherently good. Suitability depends on row counts, selectivity, memory, ordering and available indexes.

## 15. Reading a join plan

In PostgreSQL:

```sql
EXPLAIN (ANALYZE, BUFFERS)
SELECT a.id, c.full_name
FROM assignment AS a
JOIN candidate AS c
  ON c.id = a.candidate_id
WHERE a.state = 'STARTED';
```

Check:

1. estimated rows versus actual rows;
2. join algorithm and join condition;
3. scan type for each input;
4. number of loops;
5. rows removed by filters;
6. buffer hits, reads and spills;
7. total execution time.

Large estimation errors often come from stale statistics, skew, correlated columns or predicates the optimizer cannot estimate well.

## 16. Indexing join keys

Foreign-key columns are not automatically indexed in every database. For common parent-to-child lookups, index the child foreign key:

```sql
CREATE INDEX idx_assignment_candidate
  ON assignment (candidate_id);

CREATE UNIQUE INDEX uq_submission_assignment
  ON submission (assignment_id);
```

A useful composite index may combine a join key with a selective filter or ordering column:

```sql
CREATE INDEX idx_assignment_candidate_state
  ON assignment (candidate_id, state);
```

Index design must match real access patterns. Every index adds storage, write amplification and maintenance work. Verify with representative data and plans.

## 17. Production failure patterns

- Missing join predicate produces an accidental Cartesian product.
- Wrong join type silently excludes required business rows.
- A `WHERE` predicate null-rejects the outer-joined table.
- Incomplete composite-key joins multiply or misassociate rows.
- `DISTINCT` hides a modelling or cardinality defect.
- ORM-generated queries create N+1 round trips.
- Joining many wide tables causes excessive network and memory use.
- Functions or implicit casts on join keys prevent efficient access.
- Type or collation mismatches add conversions and surprising semantics.
- Row-level security changes which matches are visible.
- Pagination after a multiplying join returns unstable pages.

## 18. ORM considerations

In JPA/Hibernate, a fetch join can prevent N+1 queries, but fetching multiple collections may multiply rows and break pagination. Use projections, batch fetching, entity graphs or separate queries based on the access pattern.

In SQLAlchemy, choose joined loading, select-in loading or explicit joins deliberately. Confirm emitted SQL and query count in tests; ORM convenience does not remove database cardinality.

## Hands-on lab

Using the Online Interview Platform schema:

1. List every assignment with candidate and interview names.
2. Include assignments that have no submission.
3. Find candidates with at least one reviewed submission using `EXISTS`.
4. Find assignments without submissions using `NOT EXISTS`.
5. Rank candidates by interview without losing submission detail.
6. Report answer counts per submission without row multiplication.
7. Reconcile imported candidates with existing candidates using a full outer join.
8. Generate candidate/interview test combinations with a bounded cross join.
9. Capture plans before and after adding a justified index.
10. Create skewed data, observe an estimation error and refresh statistics.
11. Demonstrate the outer-join `ON`-vs-`WHERE` bug with tests.
12. Explain the grain and expected maximum row count for every query.

## Interview questions

1. What is the difference between `ON` and `WHERE` in an outer join?
2. Why can a one-to-many join return more rows than the left table?
3. When is `EXISTS` better than `JOIN DISTINCT`?
4. Why is `NOT EXISTS` safer than `NOT IN`?
5. How do nested-loop, hash and merge joins differ?
6. Does a foreign key automatically create an index?
7. How would you diagnose a join that became slow after data growth?
8. Why can joining two child collections produce a Cartesian multiplication?
9. When would you use a full outer join?
10. How do fetch joins interact with ORM pagination?

## Two-minute senior answer

“I begin by defining the output grain and the cardinality of every relationship. I select inner or outer semantics from the business question, use `EXISTS` for presence and `NOT EXISTS` for absence, and test null and duplicate boundary cases. For performance, I inspect estimated versus actual rows and the chosen nested-loop, hash or merge strategy, then connect any index to a measured access pattern. I do not hide unexplained multiplication with `DISTINCT`, and I verify ORM-generated SQL because application-level abstractions do not change relational semantics.”

## Completion checklist

- [ ] I can predict a join’s output grain and cardinality.
- [ ] I can explain all standard join types with a business example.
- [ ] I can use `EXISTS` and `NOT EXISTS` correctly.
- [ ] I can prevent outer-join predicate and null bugs.
- [ ] I can diagnose row multiplication.
- [ ] I can read the three main physical join algorithms in a plan.
- [ ] I can justify indexes on join paths with evidence.
- [ ] I can test joins using nulls, duplicates, missing rows and skewed data.
