# Aveli — Server PostgreSQL Physical Schema

## Таблицы

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

- один registration trial на user обеспечивается partial UNIQUE index;
- одна subscription row на `(user_id, entitlement_id)` обеспечивается UNIQUE;
- webhook idempotency поддерживается UNIQUE `subscription_events.external_event_id`;
- session expiry должна удовлетворять `expires_at > created_at`;
- access-grant time windows ограничены CHECK;
- registration trial и subscription state физически разделены.

## Access Persistence Mapping

| Access source | Physical source |
|---|---|
| Lifetime | `access_grants`, `type=lifetime` |
| Manual | `access_grants`, `type=manual_temporary` |
| Subscription | `subscriptions` |
| Registration Trial | `access_grants`, `type=trial`, `source=registration` |

Access decision algorithm относится к backend/access; этот документ владеет только persistence facts, используемыми алгоритмом.
