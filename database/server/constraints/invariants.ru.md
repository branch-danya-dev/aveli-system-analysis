# Server Database Invariants

> Product-significant invariants, enforced PostgreSQL.

| Invariant | Enforcement |
|---|---|
| Один registration trial на user | Partial UNIQUE index на `access_grants(user_id)` для registration-trial rows. |
| Одна subscription snapshot на entitlement на user | UNIQUE `(user_id, entitlement_id)`. |
| Webhook/event idempotency | UNIQUE `subscription_events.external_event_id`. |
| Valid session time window | CHECK `auth_sessions.expires_at > created_at`. |
| Valid access-grant time window | CHECK для `starts_at` / `ends_at`. |
| Lifetime не имеет end date | CHECK по access-grant type и `ends_at`. |

## Enforcement Boundary

Документ содержит **server PostgreSQL-enforced** invariants.

Application-layer validation и local SQLite constraints принадлежат своим owning documents и не должны смешиваться с server constraint set.

## Связанная документация

- [`../entities/access_grants.ru.md`](../entities/access_grants.ru.md)
- [`../entities/auth_sessions.ru.md`](../entities/auth_sessions.ru.md)
- [`../entities/subscriptions.ru.md`](../entities/subscriptions.ru.md)
- [`../entities/subscription_events.ru.md`](../entities/subscription_events.ru.md)
