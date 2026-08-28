# Server Persistence

> Проверенный PostgreSQL persistence, принадлежащий Aveli backend.

## Назначение

`server/` описывает physical persistence для identity, sessions, access grants, subscription state и subscription-event processing.

Текущие physical tables:

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

Backend не хранит professional workspace clients, services, appointments, payments, notes или photos.

## Навигация

| Область | Ответственность |
|---|---|
| `schema/` | PostgreSQL physical schema и relationships. |
| `entities/` | Table-level documentation. |
| `constraints/` | Cross-table и product-critical database invariants. |
| `migrations/` | История Prisma/PostgreSQL migrations. |

## Проверенные implementation sources

- `backend/prisma/schema.prisma`
- `backend/prisma/migrations/`
