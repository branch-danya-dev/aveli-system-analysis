# System Review — Closure Register

> Final classification of items previously tracked as open questions.

## Status

There are **no unresolved architecture/documentation blockers** for the current Aveli baseline.

## Resolved in Final Polish

| Topic | Resolution |
|---|---|
| Client archive/delete | Archive is the history-preserving path; permanent delete is allowed only when no appointment history references the client. |
| Service lifecycle | No separate deactivation state in the current product baseline; referenced services must not be removed in a way that invalidates historical appointment meaning. |
| Appointment conflict semantics | Product contract remains at the current validated level: configured scheduling rules and conflict rejection apply. Exact low-level interval algorithm is not promoted to business truth without direct implementation evidence. |
| Payment lifecycle | One aggregate payment record per appointment; partial/full progress lives in that record; duplicate independent rows are invalid current state. |
| Offline duration ownership | Business rule remains policy-based; server deadline is preferred and client 72h is an implementation default. |
| `DELETE /v1/auth/me` repeated HTTP behavior | One authenticated delete is guaranteed; repeated post-deletion HTTP delete is outside the guaranteed public contract. |
| Verification/reset auth routes | 501 stubs are future placeholders, not current shipped product capability. |
| Multi-account UI | Out of current product scope; one active account/workspace context at a time. |
| Legacy `aveli.db` claim | Helper exists but no shipped UI path; out of current flow until an explicit migration feature exists. |
| Drift ownership | Canonical in `frontend/stack/drift/`. |
| Prisma ownership | Canonical in `backend/stack/prisma/`. |
| `operations/` perspective | Not required by current Aveli ownership; release/failure synthesis remains under `system/review/`. |
| SSAD folder policy | Analytical perspectives are required; fixed directory templates are not. |

## Accepted Current Limitations

| Item | Meaning |
|---|---|
| Service duration column naming | Detailed evidence uses `duration` / `return_interval`; older overview used `_minutes`. Logical meaning is stable. |
| Canonical 429 response body | Rate limits are known; exact dedicated body is not established and is not invented. |
| Android backup behavior | No repository evidence establishes backup policy for SQLite/photos. |
| Reminder restoration across arbitrary OEM reboot | Android boot receiver exists, but OEM-independent guarantee requires device QA. |
| Certificate pinning | Not part of current security baseline; reconsider only if threat model requires it. |
| Root/jailbreak detection | Not part of current security baseline; reconsider only if threat model requires it. |

## External / Release Evidence Required Per Environment

Verify for a real production release:

- production App Store product ids and subscription group;
- Google Play product/base-plan/offer ids;
- RevenueCat dashboard product/offering linking;
- exact resolved build/minSdk evidence if needed;
- StoreKit/provider-side configuration;
- real-store purchase/restore E2E evidence;
- per-release automated test result;
- signing/provisioning/runtime-secret configuration.

Canonical checklist: [`release-readiness.md`](release-readiness.md)

## Future Product Decisions

### Export / Import Conflict Handling

Current stable contract supports user-mediated export/import. It does **not** promise automatic merge of divergent workspace copies. A future merge feature must define replace-vs-merge, duplicate identity, precedence, media behavior and schema compatibility.

### Measurable Performance Targets

Current NFRs define responsiveness expectations without unsupported numbers. Future product/release calibration may define supported device classes, expected data volumes and numeric startup/calendar/search targets.

### RevenueCat Anonymous Identity Transfer

Provider anonymous/alias/transfer semantics are outside the current Aveli product contract until future account transfer/recovery behavior depends on them.

## Architecture-Change Triggers

These are not gaps in current design; they change the system boundary and require a new analysis branch:

- multi-device professional workspace synchronization;
- cloud workspace backup/restore service;
- public online booking backend;
- shared/team workspace;
- server-driven client messaging;
- organization/employee management.

See [`../evolution/boundary-changing-features.md`](../evolution/boundary-changing-features.md).

## Closure Rule

Any future item added here must become one of:

```text
resolved decision
accepted limitation
external/release evidence
future product task
architecture-change branch
out of scope
```

No item should remain silently ambiguous.
