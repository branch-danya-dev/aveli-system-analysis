# Integrations

> Canonical documentation for Aveli boundaries with external providers, stores, OS capabilities, and third-party APIs.

## Status

**Baseline: Stable**

The integration baseline is reconciled with Flutter/backend/native evidence. Provider-dashboard and device/OEM-only facts are classified as external release/QA evidence rather than hidden architecture assumptions.

## External Integration Landscape

| Integration | Role |
|---|---|
| RevenueCat | Subscription abstraction, purchase/restore, backend reconciliation, webhooks. |
| Apple App Store | iOS store billing/subscription management. |
| Google Play | Android store billing/subscription management. |
| Device Contacts | Read-only import into Aveli clients. |
| Device Notifications | Local appointment reminders/tap navigation. |
| Device Media | Camera/gallery input for visit photos. |
| Exchange Rate API | FX lookup for local currency conversion. |
| Device Handoff | SMS/share/file picker/store-management URLs. |

Flutter ↔ Aveli Backend is an internal system interface, not an external integration. Canonical contracts live in [`../backend/api/`](../backend/api/).

Verification: [`implementation-verification.md`](implementation-verification.md)

## Documentation Rules

[`../rules.md`](../rules.md)
