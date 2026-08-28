# Aveli — Non-Functional Requirements

<p align="center"><a href="non-functional-requirements.md"><b>English</b></a> · <a href="non-functional-requirements.ru.md">Русский</a></p>

> Product-level quality expectations and constraints.

## Status

**Baseline: Stable**

## Availability and Offline

| ID | Requirement |
|---|---|
| NFR-001 | Core workspace operations remain usable without continuous network connectivity. |
| NFR-002 | Previously available workspace data remain accessible during supported offline use. |
| NFR-003 | Temporary account/access-service unavailability does not immediately block a user while previously verified access is still trusted. |
| NFR-004 | Offline workspace availability remains bounded by the current access-verification policy. |
| NFR-005 | Operations that require current account/access/subscription state clearly require connectivity. |

## Privacy

| ID | Requirement |
|---|---|
| NFR-006 | Professional workspace information remains under the user's personal workspace ownership. |
| NFR-007 | Normal use does not require professional workspace information to synchronize to a remote workspace service. |
| NFR-008 | Professional workspace information remains isolated between authenticated users. |
| NFR-009 | User-specific visit media remains isolated between workspaces. |
| NFR-010 | Logout/access expiry does not remove workspace information. |

## Security Expectations

| ID | Requirement |
|---|---|
| NFR-011 | Authentication/access handling prevents unauthorized workspace access. |
| NFR-012 | Sensitive session/access state is not exposed through normal workspace data/UI. |
| NFR-013 | The system supports secure session renewal without credentials on every continuation. |
| NFR-014 | Stale/invalid session state is not trusted indefinitely. |
| NFR-015 | User passwords are not recoverable from normal stored authentication data. |
| NFR-016 | Secrets authorizing privileged backend/external operations are not exposed to the end user. |
| NFR-017 | Privileged integration credentials remain outside client-visible configuration. |
| NFR-018 | Production account/access communication is protected in transit. |

## Access Consistency

| ID | Requirement |
|---|---|
| NFR-019 | Workspace availability uses one consistent access decision model. |
| NFR-020 | Different screens do not independently produce contradictory access decisions. |
| NFR-021 | Access state remains consistent across startup, resume, purchase, restore and refresh. |
| NFR-022 | Reinstall/local workspace reset does not create a new trial. |

## Data Integrity

| ID | Requirement |
|---|---|
| NFR-023 | Workspace changes preserve a consistent professional data state. |
| NFR-024 | Supported data-model updates preserve existing user information. |
| NFR-025 | Invalid appointment states are rejected before becoming active workspace state. |
| NFR-026 | Payment actions do not create contradictory payment states for one appointment. |
| NFR-027 | One user's information never appears in another user's active workspace. |

## Reliability and Recovery

| ID | Requirement |
|---|---|
| NFR-028 | Startup handles missing/invalid/expired account state without corrupting workspace data. |
| NFR-029 | Failed access verification does not corrupt/delete workspace information. |
| NFR-030 | Failed subscription reconciliation leaves a recoverable state. |
| NFR-031 | Unexpected application termination does not intentionally reset/recreate the workspace. |
| NFR-032 | Account switching does not attach reminders/workspace context to the wrong user. |

## Performance

| ID | Requirement |
|---|---|
| NFR-033 | Main workspace views avoid unnecessary dependency on remote-service latency. |
| NFR-034 | Calendar/daily interactions provide immediate feedback for normal personal-workspace usage. |
| NFR-035 | Client browsing/search remains responsive for expected individual-specialist data volume. |
| NFR-036 | Access verification does not unnecessarily delay startup while cached verification remains trusted. |

## Usability

| ID | Requirement |
|---|---|
| NFR-037 | Primary workflows remain understandable without unnecessary billing/account/technical complexity. |
| NFR-038 | Recurring subscription is clearly presented as recurring. |
| NFR-039 | Trial/access messaging accurately reflects current state. |
| NFR-040 | UI supports Russian and English localization. |
| NFR-041 | Visual effects/motion do not block normal workflows. |
| NFR-042 | Product respects reduced-motion preferences where supported. |

## Maintainability

| ID | Requirement |
|---|---|
| NFR-043 | Major responsibilities remain separable enough to evolve independently where practical. |
| NFR-044 | Feature responsibilities remain independently understandable/maintainable where practical. |
| NFR-045 | Account/access behavior remains conceptually separate from professional workspace behavior. |
| NFR-046 | External-system behavior remains behind explicit integration boundaries. |
| NFR-047 | Important business rules remain testable independently from visual presentation. |

## Testability and Verification

| ID | Requirement |
|---|---|
| NFR-048 | Access decisions are verifiable by automated or repeatable tests. |
| NFR-049 | Authentication/session lifecycle behavior is verifiable. |
| NFR-050 | Supported workspace migrations are verifiable against existing user data. |
| NFR-051 | Core appointment/payment rules have repeatable verification. |
| NFR-052 | Production-readiness constraints are verifiable before release. |

## Verification Ownership

Technical verification belongs to existing owners:

```text
frontend/testing/
frontend/security/
backend/security/
integrations/
system/trust/
system/review/failure-scenarios.md
system/review/release-readiness.md
```

There is no standalone `operations/` perspective in the current Aveli baseline.

## Future Measurable Calibration

The baseline does not invent numeric targets unsupported by evidence. When needed, future calibration can define supported device classes, expected data volume, startup/calendar/search latency targets, platform backup expectations, and per-release test evidence.

See [`../../system/review/open-questions.md`](../../system/review/open-questions.md).

## Related Documentation

- [`functional-requirements.md`](functional-requirements.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../../system/trust/`](../../system/trust/)
