# Aveli — Integration Boundaries

## Core Principle

External systems могут поставлять evidence, platform capabilities или transport, но не становятся автоматически owner Aveli product decisions.

Главный пример — subscription access:

```text
Apple / Google Store
        ↓
RevenueCat
        ↓
Aveli Backend Reconciliation
        ↓
Aveli Access Decision
```

RevenueCat отвечает:

> Какое subscription state признает store/provider?

Aveli отвечает:

> Может ли account открыть workspace?

## Subscription Ecosystem

```text
Aveli Mobile
   ↕ RevenueCat SDK
RevenueCat
   ↕ Apple App Store / Google Play

RevenueCat
   ↕ REST + webhook
Aveli Backend
   ↓
subscriptions + subscription_events
   ↓
Access Resolution
```

Mobile client не self-grants workspace access из `CustomerInfo`.

## Device Capability Boundaries

### Contacts

```text
Device Contacts
      ↓ read only
Aveli Client
      ↓ normalize / deduplicate
Local Client Record
```

Aveli не пишет обратно в device address book.

### Notifications

```text
Appointment
   ↓
Local Scheduler
   ↓
OS Notification Service
   ↓ tap payload
Aveli Appointment Details
```

FCM/APNs workspace push integration отсутствует.

### Media

```text
Camera / Gallery
      ↓
Aveli Picker
      ↓ copy
Aveli-owned visit-photo file
```

External media source — temporary input; copied file становится Aveli local persistence.

### Exchange Rates

```text
Aveli Client
      ↓ HTTPS GET
open.er-api.com
      ↓ rate data
Local cache
      ↓
Currency conversion workflow
```

## Device Handoff

Client может передавать данные другой OS-managed application через:

- SMS composer;
- share sheet;
- file picker;
- browser/store subscription-management URL.

Это user-mediated handoff boundaries, а не Aveli-owned cloud backends.

## Failure Isolation

External integration failure не должен удалять unrelated professional workspace data.

```text
RevenueCat unavailable
→ billing sync unavailable
→ local workspace data preserved

Exchange API unavailable
→ conversion blocked/manual fallback
→ local workspace preserved

Notification permission denied
→ reminders unavailable
→ appointments preserved

Contacts permission denied
→ import unavailable
→ existing clients preserved
```

## Ownership Rule

Provider-specific integration documentation владеет:

```text
external responsibility
Aveli-side responsibility
identity mapping
data crossing boundary
protocol / SDK
authentication / trust
failure semantics
idempotency / reconciliation
configuration
platform constraints
```

Frontend/backend implementation details остаются у своих components.
