# Server PostgreSQL Enumerations

> PostgreSQL/Prisma enum values backend persistence model.

| Enum | Values |
|---|---|
| `UserStatus` | `active`, `disabled`, `deleted` |
| `AccessGrantType` | `trial`, `lifetime`, `manual_temporary` |
| `AccessGrantSource` | `registration`, `admin`, `support` |
| `SubscriptionProvider` | `revenuecat` |
| `Store` | `app_store`, `play_store`, `unknown` |
| `SubscriptionStatus` | `trialing`, `active`, `past_due`, `cancelled`, `expired`, `grace_period`, `revoked` |

## Граница

Это physical/backend persistence values.

Их наличие не означает автоматически, что каждый value должен присутствовать в business documentation. Product meaning поднимается выше только после classification и traceability review.
