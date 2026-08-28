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

The mobile purchase result proves that the mobile billing SDK observed a provider/store result.

It does not itself become Aveli's final entitlement decision.

The backend independently reconciles provider state using the authenticated Aveli user id.

## Webhook Path

A separate asynchronous path exists:

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

Webhook event type does not directly mutate workspace access logic.

## Provider vs Product Authority

```text
Store / RevenueCat
→ provider billing evidence

Aveli Backend
→ Aveli workspace access decision
```

## Subscription Is One Access Source

Even an active provider subscription is evaluated in the wider access model:

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
