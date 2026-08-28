# Aveli — Conceptual Domain Model

> Technology-independent model of the principal information concepts used by Aveli.

## Purpose

The conceptual model explains **what information the system reasons about**.

It does not claim that every concept is implemented as a standalone database table. `Visit` and effective `Access`, for example, may have a different physical representation.

## Domain Separation

```text
Identity & Access
        +
Professional Workspace
```

The domains interact through workspace availability, but professional workspace entities are not owned by the account domain.

## Identity & Access

### Account
User identity in Aveli.

### Session
Authenticated interaction associated with an account. Session state is separate from professional workspace data.

### Subscription
Subscription-backed information relevant to workspace access.

### Access
Effective decision:

> **May the authenticated user open the workspace now?**

Possible valid sources include Lifetime, Manual Grant, Active Subscription, and Active Trial.

Access controls workspace availability; it does not own workspace entities.

## Professional Workspace

### Client
Person receiving professional services.

### Service
Type of professional work offered by the specialist.

### Appointment
Planned professional work connecting client, service where required, and scheduling context.

Important business states include `Scheduled`, `Cancelled`, `No-show`, and `Completed`.

### Visit
Conceptual meaning of completed professional work associated with an appointment.

`Visit` does not imply a standalone physical table.

### Visit Note
Professional notes associated with visit context.

### Visit Photo
Media associated with visit context. Physical storage of metadata and files is intentionally not defined here.

### Payment
Payment state associated with professional work. Completed work may be paid or outstanding.

### Schedule
Availability context used when planning appointments.

## High-Level Relationships

```text
Account
  ├── Session
  ├── Subscription
  └── Access
         │ controls availability
         ▼
Professional Workspace
  ├── Client
  │     └── Appointment
  │             ├── Service
  │             └── Visit
  │                   ├── Visit Note
  │                   ├── Visit Photo
  │                   └── Payment
  └── Schedule
```

## Modeling Principle

> **Conceptual entities describe system meaning. Physical entities describe storage. They are allowed to differ.**

## Open Modeling Areas

Revisit after finalization of:
- client permanent deletion;
- service lifecycle;
- appointment conflict rules;
- payment state transitions;
- import/export conflict behavior.

## Related Documentation

- [`../../architecture/data-ownership.md`](../../architecture/data-ownership.md)
- [`../../architecture/data-lifecycle.md`](../../architecture/data-lifecycle.md)
- [`../../../business/requirements/business-rules.md`](../../../business/requirements/business-rules.md)
