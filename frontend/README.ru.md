# Frontend

> Каноническая документация Aveli Flutter client: application architecture, client-side state, local workspace behavior, device persistence usage, navigation, access gating и mobile integrations.

## Назначение

`frontend/` объясняет **чем владеет mobile application, как разделены ее capabilities и как current Flutter implementation реализует эти responsibilities**.

Aveli frontend — local-first operational application.

Он владеет daily workspace behavior на device и координируется с backend-owned identity, access и billing authority.

## Статус

**Implementation-verified baseline in progress**

Текущая documentation основана на Flutter client `0.2.2+4`, current `lib/` tree, `pubspec.yaml`, `pubspec.lock` и test structure.

После merge нужен финальный repository audit.

## Verified Architecture

```text
Flutter Client
├── app/                     application shell, router, bootstrap helpers
├── core/                    shared infrastructure and cross-cutting concerns
└── features/                feature-first vertical slices
    ├── appointments/
    ├── auth/
    ├── bootstrap/
    ├── calendar/
    ├── clients/
    ├── payments/
    ├── reminders/
    ├── services/
    ├── settings/
    ├── subscription/
    └── today/
```

Typical feature layering:

```text
Presentation
    ↓
Providers / Controllers
    ↓
Domain
    ↓
Repositories / Data Sources
```

Не каждый feature одинаково полно использует все layers.

## Responsibility Boundary

```text
Professional Workspace
        ↓
     Frontend
        ↓
Drift / SQLite + Local Files + Device Services

Identity + Authoritative Online Access + Subscription State
        ↓
      Backend
```

Client хранит/evaluates trusted access snapshot для temporary offline operation, но не reimplements server entitlement precedence.

## Main Frontend Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Frontend responsibility и source-level architecture. |
| `stack/` | Canonical client technology knowledge. |
| `bootstrap/` | Cold-start initialization и destination resolution. |
| `navigation/` | go_router routes, shell, redirects и deep links. |
| `state/` | Riverpod container и cross-feature dependency model. |
| `auth/` | Client authentication/session behavior и workspace activation. |
| `access/` | AccessState, secure snapshot, verification policy и Access Gate. |
| `workspace/` | Operational feature structure и local workflows. |
| `storage/` | Frontend usage SQLite, files и secure storage. |
| `offline/` | Client behavior при unavailable backend/network. |
| `notifications/` | Local visit reminder lifecycle и navigation payload. |
| `billing/` | RevenueCat mobile purchase/restore и backend reconciliation. |
| `errors/` | Error transport, mapping и presentation. |
| `security/` | Frontend-specific release и trust controls. |
| `testing/` | Verified automated-test coverage boundaries. |

## Путь чтения

```text
architecture/
    ↓
stack/
    ↓
bootstrap/ + navigation/ + state/
    ↓
auth/ + access/
    ↓
workspace/ + storage/
    ↓
offline/ + notifications/ + billing/
    ↓
errors/ + security/ + testing/
```

Начать с:

[`architecture/responsibility-boundary.ru.md`](architecture/responsibility-boundary.ru.md)

Implementation reconciliation:

[`implementation-verification.ru.md`](implementation-verification.ru.md)

## Canonical Cross-Layer Ownership

- data ownership и physical schemas → [`../database/`](../database/)
- backend HTTP contracts → [`../backend/api/`](../backend/api/)
- backend authentication/access authority → [`../backend/`](../backend/)
- business behavior → [`../business/`](../business/)

Frontend documentation описывает **client usage и behavior**, а не дублирует schemas или server rules.

## Правила документации

[`../rules.ru.md`](../rules.ru.md)
