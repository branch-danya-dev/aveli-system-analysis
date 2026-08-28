# Aveli — Server PostgreSQL Physical Schema

## Tables

```text
users
auth_sessions
access_grants
subscriptions
subscription_events
```

## Relationships

```text
users 1 → 0..* auth_sessions          ON DELETE CASCADE
users 1 → 0..* access_grants          ON DELETE CASCADE
users 1 → 0..* subscriptions          ON DELETE CASCADE
users 1 → 0..* subscription_events    ON DELETE SET NULL
subscriptions 1 → 0..* subscription_events ON DELETE SET NULL
```

## Product-Critical Persistence Rules

- one registration trial per user is enforced by a partial UNIQUE index;
- one subscription row per `(user_id, entitlement_id)` is enforced by UNIQUE;
- webhook idempotency is supported by UNIQUE `subscription_events.external_event_id`;
- session expiry must satisfy `expires_at > created_at`;
- access-grant time windows are constrained;
- registration trial and subscription state are physically separate concerns.

## Access Persistence Mapping

| Access source | Physical source |
|---|---|
| Lifetime | `access_grants` with `type=lifetime` |
| Manual | `access_grants` with `type=manual_temporary` |
| Subscription | `subscriptions` |
| Registration Trial | `access_grants` with `type=trial`, `source=registration` |

The access decision algorithm itself belongs to backend/access; this document owns only the persistence facts used by that algorithm.
