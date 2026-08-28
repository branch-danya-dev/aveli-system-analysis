# Purchase, Subscription, and Workspace Entitlement

## End-to-End Purchase Flow

```text
User opens Paywall
    ↓
Flutter loads RevenueCat offerings
    ↓
User completes store purchase
    ↓
RevenueCat returns CustomerInfo
    ↓
Flutter calls POST /v1/billing/sync
    ↓
Backend queries RevenueCat subscriber state
    ↓
Backend normalizes subscription
    ↓
Backend resolves effective access
    ↓
AccessStatusView
    ↓
Frontend stores snapshot
    ↓
Access Gate opens workspace if allowed
```

## Why Two Verification Steps Exist

Mobile purchase result подтверждает provider/store result, увиденный mobile billing SDK.

Но сам по себе он не становится final Aveli entitlement decision.

Backend независимо reconciles provider state по authenticated Aveli user id.

## Webhook Path

Separate asynchronous path:

```text
RevenueCat webhook
    ↓
Aveli Backend
    ↓
event idempotency
    ↓
RevenueCat REST reconciliation
    ↓
subscription snapshot
```

Webhook event type не мутирует workspace access logic напрямую.

## Provider vs Product Authority

```text
Store / RevenueCat
→ provider billing evidence

Aveli Backend
→ Aveli workspace access decision
```

## Subscription Is One Access Source

Active subscription оценивается в общем access model:

```text
lifetime
manual
subscription
trial
```

Canonical integration:

[`../../integrations/revenuecat/`](../../integrations/revenuecat/)

Canonical backend billing:

[`../../backend/billing/`](../../backend/billing/)
