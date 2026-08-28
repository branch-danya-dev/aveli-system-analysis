# Aveli — Synthesis Review

## Result

Current lower-level documentation описывает coherent architecture:

```text
Business intent
    ↓
local-first personal workspace
    +
backend-controlled account/access
    +
external billing/device integrations
```

Major cross-layer contradiction, блокирующий system synthesis, не обнаружен.

## Strongly Aligned Decisions

### Local Workspace

Business scope, database ownership, frontend implementation и system flows согласны: professional workspace data device-local.

### Access

Business rules, backend decision logic, frontend Access Gate и RevenueCat integration согласны: backend access authoritative, access не owns workspace data.

### Trial

Business/backend models согласны: registration trial account-owned/backend-controlled.

### Billing

Frontend/backend/integrations согласны: purchase/restore проходит backend reconciliation до workspace unlock.

### Logout

Frontend + data ownership согласны: logout сохраняет professional workspace.

### External Integrations

Integrations — supporting boundaries и не становятся accidental owners Aveli product decisions.

## Known Whole-System Cleanup Items

Final polish должен reconcile:

1. legacy numbered documentation;
2. root README/navigation;
3. stale links на old numbered paths;
4. technology canonical ownership;
5. status labels (`in progress` vs `Stable`);
6. RU/EN parity;
7. duplicated canonical knowledge;
8. stale diagrams;
9. traceability links после path migration;
10. remaining explicit OPEN questions.

## Technology Ownership Cleanup

Final target:

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

Contextual usage может link across perspectives.

После final migration duplicated canonical technology docs не должны остаться.

## Legacy Structure

Numbered legacy tree удаляется только после того, как:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

содержат всю required canonical knowledge.

До delete нужен file-by-file orphan check уникального knowledge.

## Final Target

После polish repository должен читаться как система, а не как последовательность analyst artifacts.
