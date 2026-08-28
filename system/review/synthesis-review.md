# Aveli — Synthesis Review

## Result

The current lower-level documentation describes one coherent architecture:

```text
Business intent
    ↓
local-first personal workspace
    +
backend-controlled account/access
    +
external billing/device integrations
```

No major cross-layer contradiction prevents system synthesis.

## Strongly Aligned Decisions

### Local Workspace

Business scope, database ownership, frontend implementation, and system flows agree that professional workspace data remain device-local.

### Access

Business rules, backend decision logic, frontend Access Gate, and RevenueCat integration agree that backend access is authoritative and access does not own workspace data.

### Trial

Business and backend models agree that registration trial is account-owned and backend-controlled.

### Billing

Frontend, backend, and integrations agree that purchase/restore is reconciled by backend before workspace unlock.

### Logout

Frontend and data ownership agree that logout preserves professional workspace.

### External Integrations

Integrations are supporting boundaries and do not become accidental owners of Aveli product decisions.

## Known Whole-System Cleanup Items

The final polish should reconcile:

1. legacy numbered documentation;
2. root README/navigation;
3. stale links to old numbered paths;
4. technology canonical ownership;
5. status labels (`in progress` vs `Stable`);
6. RU/EN parity;
7. duplicated canonical knowledge;
8. stale diagrams;
9. traceability links after path migration;
10. remaining explicit OPEN questions.

## Technology Ownership Cleanup

Current final target:

```text
database/stack/
→ SQLite
→ PostgreSQL

frontend/stack/
→ Flutter
→ Riverpod
→ go_router
→ Drift
→ client HTTP / secure storage / mobile SDKs

backend/stack/
→ NestJS
→ REST API
→ JWT
→ Argon2id
→ Prisma
```

Contextual usage may link across perspectives.

Do not retain duplicated canonical technology documents after final migration.

## Legacy Structure

The numbered legacy tree should be removed only after:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

contain all canonical knowledge required by the repository.

Before deletion, perform a file-by-file orphan check for unique knowledge.

## Final Target

After polish, the repository should read as a system rather than as a sequence of analyst artifacts.
