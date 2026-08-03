# 1. Database Fundamentals and Selection

A database is not merely persistent storage. It enforces invariants, coordinates concurrent work, supports access paths and provides recovery guarantees.

## Core concepts

- A table models facts; a row is one fact instance; a column has a domain.
- Primary keys identify rows. Foreign keys preserve relationships.
- Constraints make invalid states unrepresentable.
- A transaction groups changes into one logical unit.
- An index trades write/storage cost for faster access.
- The optimizer chooses an execution plan from statistics and cost estimates.

## Select by workload

| Requirement | Likely starting point |
|---|---|
| Transactions, joins, constraints | PostgreSQL |
| Flexible aggregate documents | MongoDB |
| Low-latency cache, counters, ephemeral state | Redis |
| Search relevance and text analytics | Search engine |
| Analytical scans over large history | Warehouse/lakehouse |

Use polyglot persistence only when a second store solves a measured problem worth its consistency and operational cost. PostgreSQL is a strong default, not a universal answer.

## OLTP vs OLAP

OLTP favors small, selective, concurrent reads/writes. OLAP favors large scans and aggregations. Mixing both without workload isolation can make operational latency unpredictable.

## Lab

Install PostgreSQL, create a database and roles, then model candidates and interview assignments. Add primary, foreign, unique, check and not-null constraints. Attempt invalid inserts and explain which invariant rejected each one.

## Production questions

- What are the top read/write access patterns and latency targets?
- Which facts require strong consistency?
- How much data, growth, retention and availability?
- What is the recovery point objective (RPO) and recovery time objective (RTO)?
- Who operates the store and how is correctness verified?

**Interview answer:** “I begin with workload and invariants, choose the simplest store that meets them, and treat every additional database as a new consistency, security and operational boundary.”
