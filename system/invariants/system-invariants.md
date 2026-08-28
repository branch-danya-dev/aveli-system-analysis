# Aveli — System Invariants

These invariants summarize rules whose meaning spans more than one system component.

## SI-001 — Workspace Data Ownership

Professional workspace data remain owned by the user-local workspace in the current architecture.

## SI-002 — Backend Access Authority

The backend remains the authoritative online source of workspace access.

## SI-003 — Access Does Not Own Data

Access denial or expiry must not delete professional workspace data.

## SI-004 — One Workspace Context per Authenticated User

The active backend user id selects the corresponding local database and media namespace.

A different account must not expose another user's local workspace.

## SI-005 — Purchase Is Not Direct Access

A client-side store/RevenueCat purchase result must pass through backend billing reconciliation before becoming Aveli workspace access.

## SI-006 — Trial Is Account-Owned

Registration trial is backend/account-owned and cannot be reset by reinstalling the app or deleting local workspace data.

## SI-007 — Local Work Does Not Require Continuous Workspace Sync

Normal professional workspace operations do not require a cloud copy of clients/appointments/payments.

## SI-008 — Logout Preserves Workspace

Logout clears active session/access context but preserves the local professional workspace.

## SI-009 — Explicit Profile Delete Is Different

Profile/account deletion may perform destructive local workspace cleanup.

It must not be conflated with logout or access expiry.

## SI-010 — External Failure Isolation

Failure of RevenueCat, exchange-rate API, contacts permission, notifications, or temporary network access must not silently delete unrelated workspace data.

## SI-011 — Device Contact Import Creates Aveli Data

Imported contact information becomes part of an Aveli client record.

Aveli does not write changes back to the source contact in the current integration.

## SI-012 — Webhook Is Evidence, Not Access Logic

RevenueCat webhook event type does not directly grant or revoke workspace access.

The backend reconciles provider state first.

## SI-013 — Workspace Access Is Whole-Workspace

The current product does not use feature-level premium gates.

A valid access source unlocks the workspace as a whole.

## SI-014 — No Current Multi-Device Workspace Synchronization

Professional workspace records are not synchronized between devices in the current product boundary.

## Canonical Sources

Business rules:

[`../../business/requirements/business-rules.md`](../../business/requirements/business-rules.md)

Technical ownership:

- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../integrations/`](../../integrations/)
