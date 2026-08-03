# 4. PostgreSQL Hands-On

PostgreSQL combines relational integrity, rich SQL, JSON, extensions and mature operations.

## Practical toolkit

Learn `psql`, schemas, roles, identity columns, sequences, generated columns, enums versus lookup tables, arrays, `jsonb`, views, materialized views and extensions. Use `information_schema` and PostgreSQL catalogs to inspect reality.

```sql
CREATE TABLE assignment (
  id uuid PRIMARY KEY,
  candidate_id uuid NOT NULL REFERENCES candidate(id),
  interview_id uuid NOT NULL REFERENCES interview_definition(id),
  starts_at timestamptz NOT NULL,
  ends_at timestamptz NOT NULL,
  state text NOT NULL CHECK (state IN ('SCHEDULED','STARTED','SUBMITTED','EXPIRED')),
  CHECK (ends_at > starts_at)
);
```

## JSONB decision

Use `jsonb` for optional or evolving attributes queried as a document. Keep identity, relationships and critical invariants relational. Index only JSON paths used by real queries.

## Bulk and maintenance

Use `COPY` for bulk loading, transactions for batches, and analyze after material changes. Understand MVCC, dead tuples, autovacuum, checkpoints and WAL at a conceptual level before tuning them.

## Lab

Create migrations, seed realistic data, import a CSV with `COPY`, query JSONB metadata, build a materialized reporting view, and inspect table/index sizes. Demonstrate export and restore into a second database.

## Production rules

Pin supported versions, use least-privileged roles, keep application ownership separate from migration ownership, bound statement duration and connections, and test upgrades with production-like data.
