# System

> Cross-layer synthesis системы Aveli.

## Назначение

`system/` предоставляет **ближайший общий системный уровень** Aveli.

Он не заменяет canonical knowledge из:

```text
business/
database/
backend/
frontend/
integrations/
```

Вместо этого он объясняет, как эти perspectives работают вместе как одна система.

System layer владеет моделями, описывающими сразу несколько components:

```text
system context
component relationships
runtime pipelines
trust boundaries
data movement
cross-component invariants
boundary-changing evolution
system-wide open questions
```

## Статус

**Synthesis baseline in progress**

Branch построена из current implementation-verified lower-level documentation.

Она должна стать основным input для final whole-repository polish и consistency review.

## System Shape

На верхнем уровне Aveli объединяет две разные responsibility domains:

```text
Professional Workspace
        +
Account / Access
```

Они связаны workspace access control, но не делят data ownership.

```text
Independent Specialist
        ↓
   Aveli Mobile Client
      /            \
     /              \
Local Workspace     Account / Access API
     ↓                    ↓
SQLite + Files        Aveli Backend
                           ↓
                    PostgreSQL
                           ↕
                       RevenueCat
                           ↕
                Apple App Store / Google Play
```

External device services поддерживают mobile workspace:

```text
Contacts
Notifications
Camera / Gallery
Share / File Picker
Exchange Rate API
```

## Структура

```text
system/
├── README.md
├── README.ru.md
│
├── architecture/
│   ├── context.*
│   ├── component-model.*
│   ├── boundaries.*
│   └── system-map.puml
│
├── flows/
│   ├── bootstrap-and-access.*
│   ├── authentication-and-workspace.*
│   ├── purchase-and-entitlement.*
│   ├── offline-workspace.*
│   ├── logout-and-profile-delete.*
│   └── system-runtime.puml
│
├── data/
│   └── ownership-and-movement.*
│
├── trust/
│   └── trust-model.*
│
├── invariants/
│   └── system-invariants.*
│
├── evolution/
│   └── boundary-changing-features.*
│
└── review/
    ├── synthesis-review.*
    └── open-questions.*
```

## Путь чтения

```text
architecture/context
        ↓
architecture/component-model
        ↓
architecture/boundaries
        ↓
flows/
        ↓
data/
        ↓
trust/
        ↓
invariants/
        ↓
evolution/
        ↓
review/
```

## Canonicality Rule

System document может кратко повторить lower-level факт, когда это необходимо для cross-layer model, но должен ссылаться на canonical owner.

Примеры:

```text
System:
"Professional workspace data remain device-owned."

Canonical:
→ ../database/architecture/data-ownership.ru.md


System:
"Backend resolves access using lifetime → manual → subscription → trial."

Canonical:
→ ../backend/access/access-resolution.ru.md


System:
"Purchase does not directly unlock the workspace."

Canonical:
→ ../frontend/billing/
→ ../integrations/revenuecat/
→ ../backend/billing/
```

> **Storage is hierarchical. Knowledge is graph-based.**

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
