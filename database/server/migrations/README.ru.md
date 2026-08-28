# Миграции серверной PostgreSQL

> Проверенная история миграций Prisma/PostgreSQL из предоставленного описания слоя хранения.

| Миграция | Изменение |
|---|---|
| `20260823000000_init` | `users`, `auth_sessions`, устаревшая версия `subscriptions`, `subscription_events`. |
| `20260823010000_access_grants_revenuecat` | Добавлены `access_grants`; структура `subscriptions` преобразована в снимок RevenueCat; пробный период перенесён в права доступа. |
| `20260823050000_subscription_user_entitlement_unique` | Добавлено UNIQUE `(user_id, entitlement_id)`. |
| `20260825190000_entitlement_support` | Идентификатор права доступа `pro` заменён на `support`. |

Каноническое расположение реализации:

`backend/prisma/migrations/`
