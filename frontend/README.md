# Frontend

> Canonical documentation for the Aveli Flutter client and local-first professional workspace runtime.

## Status

**Baseline: Stable**

The frontend baseline is reconciled with Flutter client `0.2.2+4`, its `lib/` structure, package evidence, navigation/state/data flows, and final whole-system ownership review.

## Verified Source Shape

```text
lib/
├── app/
├── core/
└── features/
    ├── appointments/
    ├── auth/
    ├── bootstrap/
    ├── calendar/
    ├── clients/
    ├── legal/
    ├── more/
    ├── payments/
    ├── reminders/
    ├── services/
    ├── settings/
    ├── subscription/
    └── today/
```

## Responsibility Boundary

```text
Professional Workspace
        ↓
Frontend
        ↓
Drift / SQLite + Local Files + Device Services

Identity + authoritative online access
        ↓
Backend
```

The client may trust a verified access snapshot temporarily offline, but does not recreate backend entitlement precedence.

## Canonical Cross-Layer Ownership

- physical data → [`../database/`](../database/)
- backend HTTP → [`../backend/api/`](../backend/api/)
- backend auth/access authority → [`../backend/`](../backend/)
- external providers → [`../integrations/`](../integrations/)
- business behavior → [`../business/`](../business/)

Verification record: [`implementation-verification.md`](implementation-verification.md)

## Documentation Rules

[`../rules.md`](../rules.md)
