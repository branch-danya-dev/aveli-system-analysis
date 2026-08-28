# Aveli — Integration Boundaries

## Core Principle

External systems may supply evidence, platform capabilities, or transport, but they do not automatically own Aveli product decisions.

The primary example is subscription access:

```text
Apple / Google Store
        ↓
RevenueCat
        ↓
Aveli Backend Reconciliation
        ↓
Aveli Access Decision
```

RevenueCat answers:

> What subscription state does the store/provider recognize?

Aveli answers:

> May this account open the workspace?

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

The mobile client does not self-grant workspace access from `CustomerInfo`.

## Device Capability Boundaries

### Contacts

```text
Device Contacts
      ↓ read only
Aveli Client
      ↓ normalize / deduplicate
Local Client Record
```

Aveli does not write back to the device address book.

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

No FCM/APNs workspace push integration exists.

### Media

```text
Camera / Gallery
      ↓
Aveli Picker
      ↓ copy
Aveli-owned visit-photo file
```

The external media source is temporary input; the copied file becomes Aveli local persistence.

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

The client may hand data to another OS-managed application through:

- SMS composer;
- share sheet;
- file picker;
- browser/store subscription-management URL.

These are user-mediated handoff boundaries rather than cloud backends owned by Aveli.

## Failure Isolation

External integration failure must not destroy unrelated professional workspace data.

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

Provider-specific integration documentation owns:

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

Frontend/backend implementation details remain with their components.
