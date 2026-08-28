# Billing API Contracts

## `POST /v1/billing/sync`

Purpose:

Reconcile RevenueCat subscription state for the authenticated backend user and return the updated effective access state.

Auth:

```http
Authorization: Bearer <accessToken>
```

Request body:

```text
empty
```

Important trust rule:

> The client does not submit entitlement status.

Backend derives RevenueCat identity from JWT `sub` and fetches RevenueCat server-side.

Success:

```text
200 OK
```

Response:

```text
AccessStatusView
```

Canonical schema:

[`../access/contracts.md`](../access/contracts.md)

Failure:

```text
502 BILLING_SYNC_FAILED
```

when RevenueCat state cannot be verified.

## `POST /v1/webhooks/revenuecat`

Purpose:

Receive RevenueCat subscription lifecycle events for reconciliation.

Auth:

The request `Authorization` header must exactly equal configured:

```text
REVENUECAT_WEBHOOK_AUTH
```

This is not JWT bearer authentication.

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

Webhook processing is idempotent through persisted `external_event_id`.

The webhook event type is not used as the final access decision. The backend reconciles the customer through RevenueCat REST.

## Webhook Configuration Failure

Reserved/implemented error:

```text
WEBHOOK_AUTH_REQUIRED
```

when required webhook auth configuration is missing.

Internal processing:

[`../../billing/webhook-processing.md`](../../billing/webhook-processing.md)
