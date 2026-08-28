# Server Database Invariants

> Product-significant invariants, enforced PostgreSQL.

| Invariant | Enforcement |
|---|---|
| Один registration trial на user | Partial UNIQUE index на `access_grants(user_id)` для registration trial rows. |
| Одна subscription snapshot на entitlement на user | UNIQUE `(user_id, entitlement_id)`. |
| Webhook/event idempotency | UNIQUE `subscription_events.external_event_id`. |
| Valid session time window | CHECK `auth_sessions.expires_at > created_at`. |
| Valid access-grant time window | CHECK для `starts_at` / `ends_at`. |
| Lifetime не имеет end date | CHECK по access-grant type и `ends_at`. |

Не каждый invariant enforced самой DB.

Например, non-negative local payment amounts по предоставленному описанию контролируются application layer, а не DB CHECK.

Database documentation должна различать **DB-enforced** и **application-enforced** rules.
