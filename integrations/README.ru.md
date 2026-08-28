# Integrations

> Каноническая документация Aveli по boundaries с external providers, stores, OS services и third-party APIs.

## Назначение

`integrations/` объясняет **как Aveli взаимодействует с системами вне Aveli system boundary**.

Область владеет integration-level knowledge:

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

Component-local implementation остается canonical в `frontend/` или `backend/` и здесь только referenced contextually.

## Статус

**Implementation-verified baseline in progress**

Branch основана на current Flutter `0.2.2+4`, backend billing implementation, native Android/iOS project files, integration tests и current configuration evidence.

Store-dashboard-only data намеренно остаются OPEN.

## Verified External Integration Landscape

| Integration | Role |
|---|---|
| RevenueCat | Subscription abstraction, mobile purchase/restore, backend reconciliation и webhook source. |
| Apple App Store | iOS store-side billing и subscription-management environment за RevenueCat. |
| Google Play | Android store-side billing и subscription-management environment за RevenueCat. |
| Device Contacts | Read-only import в local Aveli clients. |
| Device Notifications | Local visit reminder scheduling и tap navigation. |
| Device Media | Camera/gallery input для visit photos. |
| Exchange Rate API | External FX lookup для local currency conversion. |
| Device Handoff | SMS, share sheet, file picker и subscription-management URLs. |

Thin platform plumbing вроде connectivity документируется contextually и не promoted в отдельную external-system branch.

## Internal Aveli Boundary Is Not an Integration

```text
Flutter Client
    ↕
Aveli Backend
```

— internal system interface.

Canonical contracts:

- [`../backend/api/`](../backend/api/)
- [`../frontend/`](../frontend/)

`integrations/` начинается там, где Aveli пересекает boundary provider/store/OS capability/third-party API.

## Структура

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

## Путь чтения

1. [`architecture/integration-boundaries.ru.md`](architecture/integration-boundaries.ru.md)
2. [`revenuecat/`](revenuecat/)
3. store-specific evidence
4. device/API integrations
5. [`implementation-verification.ru.md`](implementation-verification.ru.md)

## Canonical Cross-References

- backend billing → [`../backend/billing/`](../backend/billing/)
- frontend billing → [`../frontend/billing/`](../frontend/billing/)
- frontend offline/access → [`../frontend/access/`](../frontend/access/)
- frontend reminders → [`../frontend/notifications/`](../frontend/notifications/)
- frontend device usage → [`../frontend/workspace/device-integrations.ru.md`](../frontend/workspace/device-integrations.ru.md)

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
