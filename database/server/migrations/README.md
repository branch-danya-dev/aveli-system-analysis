# Server PostgreSQL Migrations

> Verified Prisma/PostgreSQL migration history from the supplied persistence description.

| Migration | Change |
|---|---|
| `20260823000000_init` | `users`, `auth_sessions`, legacy subscriptions, `subscription_events`. |
| `20260823010000_access_grants_revenuecat` | Add `access_grants`; reshape subscriptions into RevenueCat snapshot; move trial concern into grants. |
| `20260823050000_subscription_user_entitlement_unique` | UNIQUE `(user_id, entitlement_id)`. |
| `20260825190000_entitlement_support` | Change entitlement id from `pro` to `support`. |

Canonical implementation location:

`backend/prisma/migrations/`
