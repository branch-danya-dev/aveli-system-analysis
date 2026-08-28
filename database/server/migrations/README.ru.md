# Server PostgreSQL Migrations

> Проверенная история Prisma/PostgreSQL migrations из предоставленного persistence description.

| Migration | Изменение |
|---|---|
| `20260823000000_init` | `users`, `auth_sessions`, legacy subscriptions, `subscription_events`. |
| `20260823010000_access_grants_revenuecat` | `access_grants`; reshape subscriptions в RevenueCat snapshot; trial переносится в grants. |
| `20260823050000_subscription_user_entitlement_unique` | UNIQUE `(user_id, entitlement_id)`. |
| `20260825190000_entitlement_support` | Entitlement id `pro` → `support`. |

Canonical implementation location:

`backend/prisma/migrations/`
