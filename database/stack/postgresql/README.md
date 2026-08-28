# PostgreSQL

> Backend relational database for Aveli identity, access, subscription state, and event persistence.

## Role

PostgreSQL stores the backend-owned domain:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

It does not store the professional workspace.

## Why It Fits Aveli

The backend dataset is relatively small but product-critical.

Its value is in relational consistency and constraints rather than horizontal scale.

The current schema uses PostgreSQL capabilities including:

- CHECK constraints;
- partial UNIQUE indexes;
- relational foreign keys;
- transactions;
- JSONB for raw subscription-event payloads;
- concurrent backend/webhook writes.

A critical example is the partial UNIQUE index enforcing one registration trial per user.

## Why Not MongoDB for the Current Model

The backend data is strongly related and constraint-heavy.

For the current small set of relational tables, MongoDB would move important invariants from the database into application logic without providing a meaningful product advantage.

## Other SQL Engines

MySQL or server-side SQLite could model much of the same data, but migration would require careful review of:

- partial-index behavior;
- CHECK semantics;
- JSONB usage;
- concurrency characteristics;
- generated migrations.

## Dependencies

Canonical physical usage:

- [`../../server/schema/`](../../server/schema/)
- [`../../server/entities/`](../../server/entities/)
- [`../../server/constraints/`](../../server/constraints/)
- [`../../server/migrations/`](../../server/migrations/)

Consumer technology:

- [`../prisma/`](../prisma/)

## Replaceability

**Replaceability: medium.**

The backend API boundary isolates Flutter from the database engine, but changing PostgreSQL would still require:

- migration conversion;
- constraint verification;
- JSON representation changes where applicable;
- operational/deployment changes;
- regression testing of access and billing invariants.

## Selection Status

The current technical rationale is strong.

A historical formal comparison against all alternatives is not recorded in the repository.
