# Server Database Invariants

> Product-significant invariants enforced by PostgreSQL.

| Invariant | Enforcement |
|---|---|
| One registration trial per user | Partial UNIQUE index on `access_grants(user_id)` for registration-trial rows. |
| One subscription snapshot per entitlement per user | UNIQUE `(user_id, entitlement_id)`. |
| Webhook/event idempotency | UNIQUE `subscription_events.external_event_id`. |
| Valid session time window | CHECK `auth_sessions.expires_at > created_at`. |
| Valid access-grant time window | CHECK around `starts_at` / `ends_at`. |
| Lifetime has no end date | CHECK on access-grant type and `ends_at`. |

## Enforcement Boundary

This document contains **server PostgreSQL-enforced** invariants.

Application-layer validation and local SQLite constraints belong to their owning documentation and should not be mixed into this server constraint set.

## Related Documentation

- [`../entities/access_grants.md`](../entities/access_grants.md)
- [`../entities/auth_sessions.md`](../entities/auth_sessions.md)
- [`../entities/subscriptions.md`](../entities/subscriptions.md)
- [`../entities/subscription_events.md`](../entities/subscription_events.md)
