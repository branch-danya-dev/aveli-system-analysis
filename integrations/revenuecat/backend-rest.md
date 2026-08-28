# RevenueCat — Backend REST Boundary

## Gateway

Backend files include:

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

`app_user_id` is the authenticated Aveli user UUID.

The client does not submit a separate RevenueCat id to billing sync.

## Result Classification

| RevenueCat result | Backend meaning |
|---|---|
| HTTP 200 | `ok` → map subscriber to normalized snapshot. |
| HTTP 404 | `not_found`. |
| missing secret / network error / other non-OK | `unavailable`. |

There is no explicit gateway retry and no explicit fetch timeout in the verified implementation.

## Entitlement

Mapper reads:

```text
subscriber.entitlements[support]
```

with canonical logical entitlement:

```text
support
```

## Reconciliation

### `ok`

Map provider state and upsert normalized subscription state.

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

No new upsert is performed.

A prior persisted subscription row may remain in PostgreSQL until a later successful reconciliation; the failed sync does not report it as freshly verified.

## Authority Boundary

RevenueCat REST provides provider subscription evidence.

Aveli backend still runs its common access decision after reconciliation.

Backend-local implementation:

[`../../backend/billing/subscription-sync.md`](../../backend/billing/subscription-sync.md)
