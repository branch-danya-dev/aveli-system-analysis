# Frontend

> Canonical documentation for the Aveli Flutter client: application architecture, client-side state, local workspace behavior, device persistence usage, navigation, access gating, and mobile integrations.

## Purpose

`frontend/` explains **what the mobile application owns, how its capabilities are separated, and how the current Flutter implementation realizes those responsibilities**.

Aveli frontend is a local-first operational application.

It owns daily workspace behavior on the device while coordinating with backend-owned identity, access, and billing authority.

## Status

**Implementation-verified baseline in progress**

Current documentation is grounded in the Flutter client `0.2.2+4`, the current `lib/` tree, `pubspec.yaml`, `pubspec.lock`, and test structure.

A final repository audit should be performed after this package is merged.

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

Not every feature populates every layer equally.

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

The client stores and evaluates a trusted access snapshot for temporary offline operation, but it does not reimplement server entitlement precedence.

## Main Frontend Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Frontend responsibility and source-level architecture. |
| `stack/` | Canonical client technology knowledge. |
| `bootstrap/` | Cold-start initialization and destination resolution. |
| `navigation/` | go_router routes, shell, redirects and deep links. |
| `state/` | Riverpod container and cross-feature dependency model. |
| `auth/` | Client authentication/session behavior and workspace activation. |
| `access/` | AccessState, secure snapshot, verification policy and Access Gate. |
| `workspace/` | Operational feature structure and local workflows. |
| `storage/` | Frontend usage of SQLite, files and secure storage. |
| `offline/` | Client behavior under unavailable backend/network. |
| `notifications/` | Local visit reminder lifecycle and navigation payload. |
| `billing/` | RevenueCat mobile purchase/restore and backend reconciliation. |
| `errors/` | Error transport, mapping and presentation. |
| `security/` | Frontend-specific release and trust controls. |
| `testing/` | Verified automated-test coverage boundaries. |

## Reading Path

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

Start with:

[`architecture/responsibility-boundary.md`](architecture/responsibility-boundary.md)

Implementation reconciliation:

[`implementation-verification.md`](implementation-verification.md)

## Canonical Cross-Layer Ownership

- data ownership and physical schemas → [`../database/`](../database/)
- backend HTTP contracts → [`../backend/api/`](../backend/api/)
- backend authentication/access authority → [`../backend/`](../backend/)
- business behavior → [`../business/`](../business/)

Frontend documentation describes **client usage and behavior**, not duplicate schemas or server rules.

## Documentation Rules

[`../rules.md`](../rules.md)
