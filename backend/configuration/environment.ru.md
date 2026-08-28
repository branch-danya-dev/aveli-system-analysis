# Backend Environment Variables

| Variable | Default | Responsibility |
|---|---|---|
| `DATABASE_URL` | none | PostgreSQL connection. |
| `JWT_ACCESS_SECRET` | none | Access JWT signing secret. |
| `JWT_ACCESS_TTL` | `15m` | Access-token lifetime. |
| `JWT_REFRESH_TTL_DAYS` | `60` | Persisted refresh-session lifetime, days. |
| `TRIAL_DAYS` | `30` | Registration-trial duration. |
| `SUBSCRIPTION_OFFLINE_GRACE_HOURS` | `72` | Verification-window hint для client. |
| `REVENUECAT_SECRET_API_KEY` | none | Server-only RevenueCat REST credential. |
| `REVENUECAT_WEBHOOK_AUTH` | none | Exact webhook Authorization header value. |
| `REVENUECAT_API_BASE` | `https://api.revenuecat.com` | RevenueCat API base. |
| `CORS_ORIGINS` | empty | Allowed browser origins. |
| `PORT` | `3000` | Listen port. |
| `HOST` | `0.0.0.0` | Listen host. |
| `AVELI_ENV` | none | Environment label: staging / production. |

## Client API Base

Flutter client использует:

```text
AVELI_API_BASE
```

Current examples:

```text
release: https://api.aveli.app
staging: https://api-staging.aveli.app
```

Это client configuration, не NestJS env variable из основной таблицы.

## Policy vs Constant

Defaults:

```text
TRIAL_DAYS=30
SUBSCRIPTION_OFFLINE_GRACE_HOURS=72
```

являются configuration-backed behavior.

Их нельзя описывать как immutable implementation constants.

Product-level 30-day registration trial также подтвержден current business rules.

## Secret Classification

Secret:

```text
DATABASE_URL
JWT_ACCESS_SECRET
REVENUECAT_SECRET_API_KEY
REVENUECAT_WEBHOOK_AUTH
```

Non-secret/configuration:

```text
JWT_ACCESS_TTL
JWT_REFRESH_TTL_DAYS
TRIAL_DAYS
SUBSCRIPTION_OFFLINE_GRACE_HOURS
REVENUECAT_API_BASE
CORS_ORIGINS
PORT
HOST
AVELI_ENV
```

Actual secret storage/rotation зависит от deployment infrastructure и source description его не определяет.
