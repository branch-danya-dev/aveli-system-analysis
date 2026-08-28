# Boundary-Changing Features

## Purpose

Some requests look like normal features but would change foundational system decisions.

They require architecture review before implementation decomposition.

## Multi-Device Workspace Sync

Would change:

```text
device-only workspace source of truth
→ distributed / server-mediated workspace state
```

Impact:

- data ownership;
- sync/conflict model;
- backend responsibilities;
- identity/device model;
- media storage;
- offline merge behavior;
- privacy/security;
- migrations.

## Cloud Workspace Backup

A managed backup service introduces a server-held copy of professional data.

Even without live sync, this changes:

- persistence boundary;
- retention/deletion rules;
- encryption/security;
- restore semantics;
- user expectations.

## Public Online Booking

Would introduce server-side professional availability/booking data.

This changes the current rule that the backend does not store appointments/workspace entities.

## Shared Team Workspace

Would change the personal ownership model into shared ownership.

Requires:

```text
organizations
roles
permissions
shared records
concurrency
audit
```

## Server-Driven Client Messaging

Would introduce:

```text
server-side communication jobs
client-contact destinations
delivery providers
consent / notification rules
```

This is not the same as current local reminders.

## Organization / Employee Management

Would change the product itself from a personal workspace toward a multi-user operational platform.

## Rule

> A feature that changes data ownership, trust authority, or system boundary must be treated as an architecture change before it is treated as an implementation task.

Canonical product scope:

[`../../business/scope/scope.md`](../../business/scope/scope.md)
