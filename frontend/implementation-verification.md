# Frontend Implementation Verification

> Reconciliation record between prior architecture documentation and the current Flutter client implementation.

## Status

**Applied to this frontend implementation pass**

This file is an audit record. Canonical knowledge should live in the owning frontend area.

## Verified Baseline

The current client is:

```text
Aveli 0.2.2+4
Dart SDK ^3.7.0
feature-first Flutter architecture
Riverpod 2.6.1
go_router 14.8.1
Drift 2.34.3
```

The verified startup/access/workspace model is now reflected across the frontend documentation.

## Important Corrections Applied

### Legacy Database Claim

Prior documentation may imply automatic legacy `aveli.db` claim on first login.

Current source reality:

```text
LocalDatabaseManager supports claimLegacyIfPresent
BUT
no current UI/client caller enables it
```

Therefore legacy auto-claim is not documented as current shipped behavior.

### Access Authority

RevenueCat `CustomerInfo` does not directly unlock the workspace.

Current flow:

```text
purchase/restore
→ RevenueCat result
→ POST /v1/billing/sync
→ backend AccessStatusView
→ AccessState
→ Access Gate
```

### Offline Grace

The client contains a 72-hour default in `AccessVerificationPolicy`.

The backend-provided `nextVerificationRequiredAt` is preferred when present.

The documentation therefore treats 72h as a current client default, not a universal immutable product constant.

### Logout vs Profile Delete

Logout:

```text
preserve SQLite + photos
```

Profile delete:

```text
delete local SQLite + user photos + snapshot + session state
```

These lifecycles are now explicitly separated.

### Logout All

Backend supports logout-all, but no current Flutter client/UI method was found.

### Email Verification / Password Reset

Client routes/UI may exist, but current backend contract returns `501 AUTH_NOT_IMPLEMENTED`.

They are not documented as shipped end-to-end capability.

## Verified Source Mismatches / Gaps

| Topic | Current source reality |
|---|---|
| Legacy DB claim | Helper exists, no UI caller. |
| Logout all devices | Backend only, no Flutter method/UI. |
| Profile phone/email verify | Local flags only; no real SMS/email verification API. |
| Public listing/lifecycle profile fields | Local `app_settings`, no server sync. |
| Brand string `Avile` | Present in some generated/localized source; product is Aveli. |
| RC direct unlock | Not used; backend sync remains required. |

## Open Questions

- Android backup behavior for SQLite/files;
- certificate pinning;
- root/jailbreak detection;
- exact production store product ids (runtime RevenueCat configuration);
- future multi-account product behavior;
- reboot-specific reminder restoration independent of app resume;
- current full test-suite pass rate;
- real-store E2E purchase coverage.

## Baseline Recommendation

After this package is merged, run a repository-level frontend audit similar to `database/` and `backend/`.

If links, RU/EN maintenance, and ownership boundaries pass that audit, the frontend branch can be promoted to:

```text
Baseline: Stable
```
