# ADR-0001: Keep PostgreSQL as the authoritative interview system of record

- Status: Accepted
- Date: 2026-07-22
- Decision owner: Sunil Pandey
- Related system: AI-assisted online interview platform

## Context

Interview definitions, assignments, candidate answers, submissions, reviews, and published results require durable state, relational integrity, auditability, and transactional state transitions. The platform also uses Kafka for asynchronous processing and AI services for probabilistic evaluation.

## Decision drivers

- No acknowledged submission may be lost.
- Duplicate requests and events must not duplicate business transitions.
- Review and publication history must be auditable.
- Business state must not depend on browser storage.
- AI and messaging failures must not corrupt authoritative state.

## Options considered

### PostgreSQL as system of record with transactional outbox

Provides ACID transactions, constraints, indexing, mature operations, and atomic persistence of business changes with pending integration events. It requires an outbox relay and lifecycle management.

### Kafka as primary event store

Provides a durable event log and replay, but rebuilding operational state, evolving events, handling privacy deletion, and enforcing relational invariants would add unnecessary complexity at the initial scale.

### Browser/local storage plus eventual synchronization

Simplifies some UI work but cannot provide authoritative durability, multi-device correctness, secure business-state ownership, or acceptable auditability.

## Decision

Use PostgreSQL as the authoritative system of record. Use Kafka for durable asynchronous work and integration events. Persist a business transition and its outbox record in the same PostgreSQL transaction; publish and consume events idempotently.

The browser may keep temporary presentation state in memory but must not store authoritative business data, access tokens, or refresh tokens in local storage.

## Consequences

### Positive

- Strong transactional boundaries for submission and publication
- Clear ownership and recovery model
- Safe asynchronous integration through the outbox
- Familiar migration, backup, and query tooling

### Negative

- Outbox relay, cleanup, monitoring, and replay tooling are required
- PostgreSQL capacity and connection management must be engineered
- Eventual consistency remains between the database and downstream consumers

## Risks and mitigations

| Risk | Impact | Mitigation |
|---|---|---|
| Outbox backlog | Delayed evaluation | Monitor age/size, scale relay, alert on SLO |
| Duplicate delivery | Repeated side effects | Idempotency keys and consumer processing ledger |
| Schema migration failure | Deployment disruption | Compatibility rules, backups, rehearsal, roll-forward |
| Database outage | Submission unavailable | HA, bounded retries, tested recovery |

## Validation

Test concurrent submissions, duplicate API calls, Kafka downtime, relay restart, consumer redelivery, database backup restore, and migration rollback/roll-forward procedures.

## Revisit when

Reconsider if independently owned domains need separate databases, regional write requirements exceed the selected PostgreSQL topology, or an evidence-based event-sourcing use case emerges.
