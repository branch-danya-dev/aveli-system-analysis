# Backend Environment Variables

| Variable | Default | Responsibility |
|---|---|---|
| `DATABASE_URL` | none | PostgreSQL connection. |
| `JWT_ACCESS_SECRET` | none | Access JWT signing secret. |
| `JWT_ACCESS_TTL` | `15m` | Access-token lifetime. |
| `JWT_REFRESH_TTL_DAYS` | `60` | Persisted refresh-session lifetime in days. |
| `TRIAL_DAYS` | `30` | Registration-trial duration. |
| `SUBSCRIPTION_OFFLINE_GRACE_HOURS` | `72` | Verification-window hint returned to the client. |
| `REVENUECAT_SECRET_API_KEY` | none | Server-only RevenueCat REST credential. |
| `REVENUECAT_WEBHOOK_AUTH` | none | Exact webhook Authorization header value. |
| `REVENUECAT_API_BASE` | `https://api.revenuecat.com` | RevenueCat API base. |
| `CORS_ORIGINS` | empty | Allowed browser origins when CORS is enabled/configured. |
| `PORT` | `3000` | Listen port. |
| `HOST` | `0.0.0.0` | Listen host. |
| `AVELI_ENV` | none | Environment label such as staging / production. |

## Client API Base

The Flutter client uses:

```text
AVELI_API_BASE
```

Current environment examples:

```text
release: https://api.aveli.app
staging: https://api-staging.aveli.app
```

This is client configuration rather than a NestJS environment variable in the table above.

## Policy vs Constant

Defaults such as:

```text
TRIAL_DAYS=30
SUBSCRIPTION_OFFLINE_GRACE_HOURS=72
```

are configuration-backed behavior.

Documentation should not describe them as immutable implementation constants.

Product-level 30-day registration trial is additionally supported by current business rules.

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

Actual secret storage and rotation procedures depend on deployment infrastructure and are outside the current evidence.
