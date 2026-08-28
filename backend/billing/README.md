# Backend Billing

> Server-side RevenueCat reconciliation and subscription-state processing.

## Purpose

`billing/` explains how Aveli converts external subscription evidence into normalized backend subscription state.

Billing does not directly bypass the common access decision.

## Components

Verified implementation:

```text
BillingModule
├── BillingController
├── SubscriptionSyncService
├── RevenueCatWebhookService
└── HttpRevenueCatGateway
```

## Navigation

- [`subscription-sync.md`](subscription-sync.md)
- [`webhook-processing.md`](webhook-processing.md)
- [`revenuecat-mapping.md`](revenuecat-mapping.md)
- [`billing-flow.puml`](billing-flow.puml)

Canonical HTTP contracts:

[`../api/billing/`](../api/billing/)

Provider-specific RevenueCat integration belongs to the system integration area:

```text
../../integrations/revenuecat/
```

when that target area is migrated.
