# Server PostgreSQL Enumerations

> PostgreSQL/Prisma enum values used by the backend persistence model.

| Enum | Values |
|---|---|
| `UserStatus` | `active`, `disabled`, `deleted` |
| `AccessGrantType` | `trial`, `lifetime`, `manual_temporary` |
| `AccessGrantSource` | `registration`, `admin`, `support` |
| `SubscriptionProvider` | `revenuecat` |
| `Store` | `app_store`, `play_store`, `unknown` |
| `SubscriptionStatus` | `trialing`, `active`, `past_due`, `cancelled`, `expired`, `grace_period`, `revoked` |

## Boundary

These are physical/backend persistence values.

Their existence does not automatically imply that every value belongs in business documentation. Product meaning is promoted upward only after classification and traceability review.
