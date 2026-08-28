# Aveli — System Invariants

Эти invariants суммируют rules, смысл которых пересекает несколько system components.

## SI-001 — Workspace Data Ownership

Professional workspace data остаются owned user-local workspace в current architecture.

## SI-002 — Backend Access Authority

Backend остается authoritative online source workspace access.

## SI-003 — Access Does Not Own Data

Access denial/expiry не должны удалять professional workspace data.

## SI-004 — One Workspace Context per Authenticated User

Active backend user id выбирает соответствующий local database и media namespace.

Другой account не должен видеть чужой local workspace.

## SI-005 — Purchase Is Not Direct Access

Client-side store/RevenueCat purchase result должен пройти backend billing reconciliation до превращения в Aveli workspace access.

## SI-006 — Trial Is Account-Owned

Registration trial backend/account-owned и не reset через reinstall или delete local workspace.

## SI-007 — Local Work Does Not Require Continuous Workspace Sync

Normal professional operations не требуют cloud copy clients/appointments/payments.

## SI-008 — Logout Preserves Workspace

Logout очищает active session/access context, но сохраняет local professional workspace.

## SI-009 — Explicit Profile Delete Is Different

Profile/account deletion может выполнять destructive local cleanup.

Его нельзя смешивать с logout/access expiry.

## SI-010 — External Failure Isolation

Failure RevenueCat, exchange-rate API, contacts permission, notifications или temporary network не должен silently delete unrelated workspace data.

## SI-011 — Device Contact Import Creates Aveli Data

Imported contact information становится частью Aveli client record.

Aveli не пишет изменения обратно в source contact.

## SI-012 — Webhook Is Evidence, Not Access Logic

RevenueCat webhook event type не grants/revokes workspace access напрямую.

Backend сначала reconciles provider state.

## SI-013 — Workspace Access Is Whole-Workspace

Current product не использует feature-level premium gates.

Valid access source unlocks workspace whole.

## SI-014 — No Current Multi-Device Workspace Synchronization

Professional workspace records не synchronized между devices в current product boundary.

## Canonical Sources

Business rules:

[`../../business/requirements/business-rules.ru.md`](../../business/requirements/business-rules.ru.md)

Technical ownership:

- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../integrations/`](../../integrations/)
