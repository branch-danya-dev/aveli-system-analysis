# Billing API Contracts

## `POST /v1/billing/sync`

Purpose:

Reconcile RevenueCat subscription state для authenticated backend user и вернуть updated effective access state.

Auth:

```http
Authorization: Bearer <accessToken>
```

Request body:

```text
empty
```

Ключевое trust rule:

> Client не передает entitlement status.

Backend получает RevenueCat identity из JWT `sub` и делает server-side lookup.

Success:

```text
200 OK
```

Response:

```text
AccessStatusView
```

Canonical schema:

[`../access/contracts.ru.md`](../access/contracts.ru.md)

Failure:

```text
502 BILLING_SYNC_FAILED
```

если RevenueCat state нельзя проверить.

## `POST /v1/webhooks/revenuecat`

Purpose:

Принимать RevenueCat subscription lifecycle events для reconciliation.

Auth:

Request `Authorization` header должен точно равняться configured:

```text
REVENUECAT_WEBHOOK_AUTH
```

Это не JWT bearer authentication.

Body:

```text
arbitrary RevenueCat webhook JSON
```

Success:

```json
{
  "ok": true
}
```

Webhook processing idempotent через persisted `external_event_id`.

Webhook event type не является final access decision. Backend reconciles customer через RevenueCat REST.

## Webhook Configuration Failure

Error:

```text
WEBHOOK_AUTH_REQUIRED
```

когда required webhook auth configuration отсутствует.

Internal processing:

[`../../billing/webhook-processing.ru.md`](../../billing/webhook-processing.ru.md)
