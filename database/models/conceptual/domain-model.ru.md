# Aveli — Conceptual Domain Model

> Technology-independent модель основных information concepts Aveli.

## Назначение

Conceptual model объясняет **с какой информацией работает система на уровне смысла**.

Она не утверждает, что каждая концепция реализована отдельной database table. `Visit` и effective `Access`, например, могут иметь другое physical representation.

## Разделение доменов

```text
Identity & Access
        +
Professional Workspace
```

Домены взаимодействуют через workspace availability, но professional workspace entities не принадлежат account domain.

## Identity & Access

### Account
Identity пользователя Aveli.

### Session
Authenticated interaction, связанный с account. Session state отделен от professional workspace data.

### Subscription
Subscription-backed information, значимая для workspace access.

### Access
Effective decision:

> **Может ли аутентифицированный пользователь открыть workspace сейчас?**

Valid sources: Lifetime, Manual Grant, Active Subscription, Active Trial.

Access управляет workspace availability, но не владеет workspace entities.

## Professional Workspace

### Client
Человек, получающий профессиональные услуги.

### Service
Тип профессиональной услуги специалиста.

### Appointment
Запланированная профессиональная работа, связывающая client, service там, где он требуется, и scheduling context.

Значимые business states: `Scheduled`, `Cancelled`, `No-show`, `Completed`.

### Visit
Концептуальный смысл завершенной профессиональной работы, связанной с appointment.

`Visit` не предполагает обязательную standalone physical table.

### Visit Note
Professional notes, связанные с visit context.

### Visit Photo
Media, связанные с visit context. Physical storage metadata и files здесь намеренно не определяется.

### Payment
Payment state, связанный с professional work. Completed work может быть paid или outstanding.

### Schedule
Availability context для планирования appointments.

## Верхнеуровневые связи

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

## Принцип моделирования

> **Conceptual entities описывают смысл системы. Physical entities описывают storage. Они могут различаться.**

## Открытые области

Пересмотреть после финализации:
- client permanent deletion;
- service lifecycle;
- appointment conflict rules;
- payment state transitions;
- import/export conflict behavior.

## Связанная документация

- [`../../architecture/data-ownership.ru.md`](../../architecture/data-ownership.ru.md)
- [`../../architecture/data-lifecycle.ru.md`](../../architecture/data-lifecycle.ru.md)
- [`../../../business/requirements/business-rules.ru.md`](../../../business/requirements/business-rules.ru.md)
