# Aveli — System Context

## Product Context

Aveli is a personal mobile workspace for an independent beauty professional.

Its operating model is intentionally narrow:

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

The system is designed as a lightweight professional workspace rather than a salon-management platform or enterprise CRM.

Canonical business context:

[`../../business/context/product-context.md`](../../business/context/product-context.md)

## Primary Actor

```text
Independent Beauty Professional
```

The specialist uses Aveli to:

- understand the current day;
- manage clients;
- plan appointments;
- maintain services;
- record visit context;
- track payments;
- receive local reminders;
- manage workspace access.

## System Boundary

Aveli itself includes:

```text
Aveli Mobile Client
Aveli Backend
Aveli-controlled persistence
Aveli integration logic
```

The following are external:

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

Aveli is deliberately divided into:

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

This is the specialist's operational data.

The current system keeps it device-local.

### Identity and Access

```text
account
session
trial
manual/lifetime grants
subscription state
effective access decision
```

This is backend-controlled.

## Important Product Boundary

Current Aveli does **not** provide:

```text
cloud workspace synchronization
multi-device workspace synchronization
shared team workspace
public online booking backend
server-side client CRM
server-side appointment storage
```

These are architecture-changing exclusions, not merely missing UI features.

Canonical scope:

[`../../business/scope/scope.md`](../../business/scope/scope.md)
