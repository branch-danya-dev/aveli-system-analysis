# Boundary-Changing Features

## Purpose

Некоторые requests выглядят обычными features, но меняют foundational system decisions.

Для них нужен architecture review до implementation decomposition.

## Multi-Device Workspace Sync

Меняет:

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

Managed backup вводит server-held copy professional data.

Даже без live sync это меняет:

- persistence boundary;
- retention/deletion rules;
- encryption/security;
- restore semantics;
- user expectations.

## Public Online Booking

Вводит server-side professional availability/booking data.

Это меняет current rule: backend не stores appointments/workspace entities.

## Shared Team Workspace

Personal ownership превращается в shared ownership.

Нужны:

```text
organizations
roles
permissions
shared records
concurrency
audit
```

## Server-Driven Client Messaging

Вводит:

```text
server-side communication jobs
client-contact destinations
delivery providers
consent / notification rules
```

Это не то же самое, что current local reminders.

## Organization / Employee Management

Меняет product model от personal workspace к multi-user operational platform.

## Rule

> Feature, меняющий data ownership, trust authority или system boundary, должен сначала рассматриваться как architecture change, а уже потом как implementation task.

Canonical scope:

[`../../business/scope/scope.ru.md`](../../business/scope/scope.ru.md)
