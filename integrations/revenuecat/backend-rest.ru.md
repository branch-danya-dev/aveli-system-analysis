# RevenueCat — Backend REST Boundary

## Gateway

Backend files:

```text
backend/src/billing/revenuecat/revenuecat.gateway.ts
backend/src/billing/revenuecat/revenuecat.mapper.ts
backend/src/billing/subscription-sync.service.ts
```

## Endpoint

Base:

```text
REVENUECAT_API_BASE
default: https://api.revenuecat.com
```

Lookup:

```http
GET /v1/subscribers/{app_user_id}
Authorization: Bearer <REVENUECAT_SECRET_API_KEY>
```

`app_user_id` — authenticated Aveli user UUID.

Client не передает отдельный RevenueCat id в billing sync.

## Result Classification

| RevenueCat result | Backend meaning |
|---|---|
| HTTP 200 | `ok` → map subscriber в normalized snapshot. |
| HTTP 404 | `not_found`. |
| missing secret / network error / other non-OK | `unavailable`. |

Verified implementation не имеет explicit gateway retry и explicit fetch timeout.

## Entitlement

Mapper читает:

```text
subscriber.entitlements[support]
```

Canonical entitlement:

```text
support
```

## Reconciliation

### `ok`

Map provider state + upsert normalized subscription state.

### `not_found`

Upsert:

```text
status = expired
```

### `unavailable`

Fail closed:

```text
502 BILLING_SYNC_FAILED
```

Новый upsert не выполняется.

Prior persisted subscription row может остаться в PostgreSQL до successful reconciliation; failed sync не сообщает его как freshly verified.

## Authority Boundary

RevenueCat REST дает provider subscription evidence.

Aveli backend после reconciliation выполняет common access decision.

Backend-local implementation:

[`../../backend/billing/subscription-sync.ru.md`](../../backend/billing/subscription-sync.ru.md)
