# Server Persistence

> Verified PostgreSQL persistence owned by the Aveli backend.

## Purpose

`server/` documents the physical persistence for identity, sessions, access grants, subscription state, and subscription-event processing.

Current physical tables:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

The backend does not persist professional workspace clients, services, appointments, payments, notes, or photos.

## Navigation

| Area | Responsibility |
|---|---|
| `schema/` | PostgreSQL physical schema and relationships. |
| `entities/` | Table-level documentation. |
| `constraints/` | Cross-table and product-critical database invariants. |
| `migrations/` | Prisma/PostgreSQL migration history. |

## Verified Implementation Sources

- `backend/prisma/schema.prisma`
- `backend/prisma/migrations/`
