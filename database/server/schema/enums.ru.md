# Перечисления серверной PostgreSQL

> Значения перечислений PostgreSQL/Prisma, используемые в серверной модели хранения.

| Перечисление | Значения |
|---|---|
| `UserStatus` | `active`, `disabled`, `deleted` |
| `AccessGrantType` | `trial`, `lifetime`, `manual_temporary` |
| `AccessGrantSource` | `registration`, `admin`, `support` |
| `SubscriptionProvider` | `revenuecat` |
| `Store` | `app_store`, `play_store`, `unknown` |
| `SubscriptionStatus` | `trialing`, `active`, `past_due`, `cancelled`, `expired`, `grace_period`, `revoked` |

## Граница модели

Это физические значения серверной модели хранения.

Их наличие не означает, что каждое значение автоматически должно присутствовать в бизнес-документации. Значение для продукта поднимается на более высокий уровень только после классификации и проверки трассируемости.
