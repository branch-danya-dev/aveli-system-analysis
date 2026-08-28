# Aveli — System Context

## Product Context

Aveli — personal mobile workspace для independent beauty professional.

Operating model намеренно узкий:

```text
One specialist
    ↓
Own clients
    ↓
Own schedule
    ↓
Own services
    ↓
Own visits
    ↓
Own payments
```

System designed как lightweight professional workspace, а не salon-management platform или enterprise CRM.

Canonical business context:

[`../../business/context/product-context.ru.md`](../../business/context/product-context.ru.md)

## Primary Actor

```text
Independent Beauty Professional
```

Specialist использует Aveli чтобы:

- понимать current day;
- управлять clients;
- планировать appointments;
- поддерживать services;
- сохранять visit context;
- отслеживать payments;
- получать local reminders;
- управлять workspace access.

## System Boundary

В Aveli входят:

```text
Aveli Mobile Client
Aveli Backend
Aveli-controlled persistence
Aveli integration logic
```

External:

```text
RevenueCat
Apple App Store
Google Play
Device Contacts
OS Notification Service
Camera / Gallery
Exchange Rate API
OS Share / File Picker / SMS / Browser
```

## Core Responsibility Split

Aveli разделен на:

### Professional Workspace

```text
clients
services
appointments
payments
notes
photos
schedule
workspace preferences
```

Это operational data specialist.

Current system хранит их device-local.

### Identity and Access

```text
account
session
trial
manual/lifetime grants
subscription state
effective access decision
```

Это backend-controlled domain.

## Important Product Boundary

Current Aveli **не** предоставляет:

```text
cloud workspace synchronization
multi-device workspace synchronization
shared team workspace
public online booking backend
server-side client CRM
server-side appointment storage
```

Это architecture-changing exclusions, а не просто missing UI features.

Canonical scope:

[`../../business/scope/scope.ru.md`](../../business/scope/scope.ru.md)
