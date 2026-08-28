# Frontend Implementation Verification

> Final reconciliation record for the current Flutter client.

## Status

**Applied — Frontend baseline Stable**

Verified baseline: Aveli `0.2.2+4`, Dart `^3.7.0`, feature-first/hybrid architecture, Riverpod 2.6.1, go_router 14.8.1, Drift 2.34.3.

## Applied Corrections

- legacy `aveli.db` claim helper exists, but no current shipped UI invokes it;
- RevenueCat `CustomerInfo` does not directly unlock workspace;
- server verification deadline is preferred; 72h remains a client implementation default where applicable;
- logout preserves SQLite/photos while explicit profile deletion performs local destructive cleanup;
- backend logout-all exists but current Flutter UI/client does not expose it;
- email verification/password-reset routes are backend 501 stubs;
- verified source tree includes `legal/` and `more/`;
- Drift canonical ownership is `frontend/stack/drift/`.

## Classified Non-Blocking Evidence Items

| Item | Classification |
|---|---|
| Android backup behavior | Platform-policy evidence not established; no claim made. |
| Certificate pinning | Not part of current security baseline; revisit only if threat model requires it. |
| Root/jailbreak detection | Not part of current security baseline; revisit only if threat model requires it. |
| Production store product ids | External release configuration evidence. |
| Multi-account UI | Out of current product scope; one active account/workspace context at a time. |
| Reboot reminder guarantee | Platform/OEM QA limitation; boot receiver exists and app-resume rebuild is verified. |
| Full test-suite pass rate | Per-release verification evidence, not architecture truth. |
| Real-store E2E purchase coverage | Future release evidence. |
| Legacy DB claim UI | Out of current shipped flow until an explicit migration feature is introduced. |

No item above blocks the stable frontend architecture baseline.
