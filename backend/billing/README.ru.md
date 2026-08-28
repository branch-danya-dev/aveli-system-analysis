# Backend Billing

> Server-side RevenueCat reconciliation и subscription-state processing.

## Назначение

`billing/` объясняет, как Aveli преобразует external subscription evidence в normalized backend subscription state.

Billing не обходит common access decision напрямую.

## Components

Verified implementation:

```text
BillingModule
├── BillingController
├── SubscriptionSyncService
├── RevenueCatWebhookService
└── HttpRevenueCatGateway
```

## Навигация

- [`subscription-sync.ru.md`](subscription-sync.ru.md)
- [`webhook-processing.ru.md`](webhook-processing.ru.md)
- [`revenuecat-mapping.ru.md`](revenuecat-mapping.ru.md)
- [`billing-flow.puml`](billing-flow.puml)

Canonical HTTP contracts:

[`../api/billing/`](../api/billing/)

Provider-specific RevenueCat integration принадлежит system integration area:

```text
../../integrations/revenuecat/
```

после миграции target area.
