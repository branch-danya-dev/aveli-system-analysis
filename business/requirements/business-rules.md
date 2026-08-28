# Aveli — Business Rules

<p align="center"><a href="business-rules.md"><b>English</b></a> · <a href="business-rules.ru.md">Русский</a></p>

> Product-level invariants and lifecycle constraints independent of implementation.

## Status

**Baseline: Stable**

`BR-057`–`BR-061` remain intentionally retired historical identifiers from the removed release-rule model and are not reused.

## Access Rules

| ID | Rule |
|---|---|
| BR-001 | A user may open the workspace only when at least one valid access source exists. |
| BR-002 | Access sources are evaluated in the priority Lifetime → Manual Grant → Active Subscription → Active Trial → None. |
| BR-003 | Lifetime access has priority over all other access sources. |
| BR-004 | Manual access has priority over subscription and trial access. |
| BR-005 | Active subscription access has priority over active trial access. |
| BR-006 | If no valid access source exists, the workspace remains unavailable. |
| BR-007 | Workspace access is granted or denied as a whole; the current product has no independent feature-level premium gates. |

## Trial Rules

| ID | Rule |
|---|---|
| BR-008 | A new account receives one 30-day registration trial. |
| BR-009 | Trial state belongs to the account, not to local installation state. |
| BR-010 | Logout does not restart or extend the trial. |
| BR-011 | Reinstalling the application does not create a new trial for the same account. |
| BR-012 | Deleting local workspace data does not create a new trial for the same account. |
| BR-013 | The current product does not combine the Aveli registration trial with a second store-managed free trial. |

## Subscription Rules

| ID | Rule |
|---|---|
| BR-014 | Monthly and yearly subscriptions grant the same logical workspace access level. |
| BR-015 | Subscription prices shown to the user reflect platform/provider pricing rather than an independently maintained static value. |
| BR-016 | A recurring store subscription is presented as recurring. |
| BR-017 | Subscription management and cancellation are performed through the corresponding mobile platform. |
| BR-018 | Completion of a client purchase flow does not bypass the common Aveli access decision. |
| BR-019 | Restoring an existing valid subscription restores subscription-based workspace access after reconciliation. |

## Workspace Data Ownership

| ID | Rule |
|---|---|
| BR-020 | Clients, appointments, services, payments, visit notes, and visit photos belong to the professional workspace domain. |
| BR-021 | Professional workspace information is not synchronized between devices in the current product model. |
| BR-022 | Account/access responsibilities remain separate from normal professional workspace ownership. |
| BR-023 | Each user has an isolated professional workspace. |
| BR-024 | User-specific workspace materials must not be exposed across different user workspaces. |
| BR-025 | Logout must not delete persistent professional workspace information. |
| BR-026 | Expiration of trial, subscription, or another access source must not delete or modify existing professional workspace information. |

## Appointment Rules

| ID | Rule |
|---|---|
| BR-027 | An appointment belongs to a valid client. |
| BR-028 | An appointment references a valid service when service selection is required by the workflow. |
| BR-029 | Appointment date/time must satisfy the configured scheduling rules. |
| BR-030 | Conflicting appointments are rejected according to the current slot-availability model. |
| BR-031 | A cancelled appointment is not treated as an active scheduled visit. |
| BR-032 | A no-show remains distinguishable from cancelled and completed visits. |
| BR-033 | A completed visit may preserve notes, photos, and payment information. |

## Payment Rules

| ID | Rule |
|---|---|
| BR-034 | Payment may be recorded only for work valid for payment under the current visit lifecycle. |
| BR-035 | A completed visit may remain unpaid. |
| BR-036 | Outstanding payments remain visible until resolved. |
| BR-037 | Payment state remains consistent with the related visit state. |
| BR-038 | Financial summaries are based on payment information recorded in the professional workspace. |

## Client Rules

| ID | Rule |
|---|---|
| BR-039 | Client records belong only to the active user's professional workspace. |
| BR-040 | Archiving a client preserves historical information. |
| BR-041 | Importing a device contact creates or enriches an Aveli client and does not modify the original device contact. |
| BR-042 | Client history is derived from professional activity associated with that client. |

## Session and Account Switching

| ID | Rule |
|---|---|
| BR-043 | Logout ends the active authenticated session. |
| BR-044 | After logout the previous workspace remains inactive until that user authenticates again. |
| BR-045 | User-specific reminders for the outgoing account are deactivated on logout. |
| BR-046 | Logout does not remove the outgoing user's persistent workspace information. |
| BR-047 | After another account becomes active, previous-user information is not exposed in the new active workspace. |

## Offline Access

| ID | Rule |
|---|---|
| BR-048 | A previously verified access state may temporarily allow offline workspace access. |
| BR-049 | Offline access is valid only within the current verification policy. |
| BR-050 | When offline verification is no longer sufficient, renewed verification is required. |
| BR-051 | Normal professional workspace operations do not require continuous network connectivity. |
| BR-052 | Operations requiring current account/access/subscription verification require connectivity. |

## Reminder Rules

| ID | Rule |
|---|---|
| BR-053 | Appointment reminders belong to appointments in the active user workspace. |
| BR-054 | Reminders must not expose another user's appointment information after account switching. |
| BR-055 | Logout deactivates reminders associated with the outgoing user. |
| BR-056 | Opening a valid reminder navigates to the related appointment when it still exists and is available. |

## Final Lifecycle Clarifications

| ID | Rule |
|---|---|
| BR-062 | Permanent client deletion is allowed only when no appointment history references that client; otherwise the history-preserving lifecycle is archive. |
| BR-063 | Archiving is the normal history-preserving way to remove a client from the active directory; restoring the client makes it active again. |
| BR-064 | A service referenced by existing appointments must be preserved so historical appointment meaning is not invalidated. |
| BR-065 | The current product does not define a separate service-deactivation state; a service is either available or safely deleted according to its references. |
| BR-066 | Appointment start/end must fit the configured working schedule used by the current workspace. |
| BR-067 | Creating or rescheduling an appointment must be rejected when its active scheduled interval conflicts with another active scheduled appointment under the current slot model. |
| BR-068 | Cancelled appointments are not active scheduled work and therefore do not participate as active conflicts. |
| BR-069 | The current payment model maintains at most one aggregate payment record for one appointment. |
| BR-070 | An appointment payment may progress through unpaid, partial, and paid states inside the same aggregate payment record. |
| BR-071 | Creating a second independent payment record for the same appointment is not a valid current product state. |
| BR-072 | Export/import is a user-mediated transfer capability; automatic merge of divergent workspace copies is not part of the current stable product contract. |

## Technical Ownership

- access resolution → [`../../backend/access/`](../../backend/access/)
- local persistence → [`../../database/local/`](../../database/local/)
- client behavior → [`../../frontend/`](../../frontend/)
- external billing → [`../../integrations/revenuecat/`](../../integrations/revenuecat/)
- cross-system trust/failure/release → [`../../system/`](../../system/)

## Related Documentation

- [`functional-requirements.md`](functional-requirements.md)
- [`non-functional-requirements.md`](non-functional-requirements.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../traceability/`](../traceability/)
