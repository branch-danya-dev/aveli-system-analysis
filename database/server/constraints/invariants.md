# Server Database Invariants

> Product-significant invariants enforced by PostgreSQL.

| Invariant | Enforcement |
|---|---|
| One registration trial per user | Partial UNIQUE index on `access_grants(user_id)` for registration trial rows. |
| One subscription snapshot per entitlement per user | UNIQUE `(user_id, entitlement_id)`. |
| Webhook/event idempotency | UNIQUE `subscription_events.external_event_id`. |
| Valid session time window | CHECK `auth_sessions.expires_at > created_at`. |
| Valid access-grant time window | CHECK around `starts_at` / `ends_at`. |
| Lifetime has no end date | CHECK on access-grant type and `ends_at`. |

Not every invariant is enforced by the database.

For example, non-negative local payment amounts are currently described as application-layer enforcement rather than DB CHECK constraints.

Database documentation must distinguish **DB-enforced** from **application-enforced** rules.
