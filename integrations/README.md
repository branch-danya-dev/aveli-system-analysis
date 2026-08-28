# Integrations

> Canonical documentation for Aveli boundaries with external providers, stores, OS services, and third-party APIs.

## Purpose

`integrations/` explains **how Aveli interacts with systems outside the Aveli system boundary**.

It owns integration-level knowledge:

```text
external party
    ↓
purpose
    ↓
data crossing boundary
    ↓
identity / trust
    ↓
failure semantics
    ↓
Aveli consumers
```

Component-local implementation remains canonical in `frontend/` or `backend/` and is referenced contextually.

## Status

**Implementation-verified baseline in progress**

This branch is grounded in the current Flutter `0.2.2+4`, backend billing implementation, native Android/iOS project files, integration tests, and current configuration evidence.

Store-dashboard-only data remains intentionally OPEN.

## Verified External Integration Landscape

| Integration | Role |
|---|---|
| RevenueCat | Subscription abstraction, mobile purchase/restore, backend reconciliation, webhook source. |
| Apple App Store | iOS store-side billing and subscription-management environment behind RevenueCat. |
| Google Play | Android store-side billing and subscription-management environment behind RevenueCat. |
| Device Contacts | Read-only import into local Aveli clients. |
| Device Notifications | Local visit reminder scheduling and tap navigation. |
| Device Media | Camera/gallery input for visit photos. |
| Exchange Rate API | External FX lookup for local currency conversion. |
| Device Handoff | SMS, share sheet, file picker, and subscription-management URLs. |

Thin platform plumbing such as connectivity is documented contextually rather than promoted to a full external-system branch.

## Internal Aveli Boundary Is Not an Integration

```text
Flutter Client
    ↕
Aveli Backend
```

is an internal system interface.

Canonical contracts:

- [`../backend/api/`](../backend/api/)
- [`../frontend/`](../frontend/)

`integrations/` begins when Aveli crosses into a provider, store, OS capability, or third-party API.

## Structure

```text
integrations/
├── architecture/
├── revenuecat/
├── app-store/
├── google-play/
├── exchange-rate/
├── device-contacts/
├── device-notifications/
├── device-media/
└── device-handoff/
```

## Reading Path

1. [`architecture/integration-boundaries.md`](architecture/integration-boundaries.md)
2. [`revenuecat/`](revenuecat/)
3. store-specific evidence
4. device/API integrations
5. [`implementation-verification.md`](implementation-verification.md)

## Canonical Cross-References

- backend billing → [`../backend/billing/`](../backend/billing/)
- frontend billing → [`../frontend/billing/`](../frontend/billing/)
- frontend offline/access → [`../frontend/access/`](../frontend/access/)
- frontend reminders → [`../frontend/notifications/`](../frontend/notifications/)
- frontend device usage → [`../frontend/workspace/device-integrations.md`](../frontend/workspace/device-integrations.md)

## Documentation Rules

[`../rules.md`](../rules.md)
