# Aveli — Non-Functional Requirements

<p align="center">
  <a href="non-functional-requirements.md"><b>English</b></a> ·
  <a href="non-functional-requirements.ru.md">Русский</a>
</p>

> Defines the quality expectations and product-level constraints that shape how Aveli must behave.

---

## Overview

Non-functional requirements describe **how well the system must behave and which quality characteristics must be preserved**.

They do not define implementation technologies.

Example:

```text
The professional workspace should remain usable without permanent network connectivity.
```

This belongs here.

Implementation details such as:

```text
SQLite
JWT
HTTPS
Docker
specific timeout values
release environment variables
```

belong in the relevant technical documentation.

---

## Availability and Offline Operation

| ID | Requirement |
|---|---|
| NFR-001 | Core professional workspace operations must remain usable without continuous network connectivity. |
| NFR-002 | Previously available clients, appointments, services, payments, notes, and visit media must remain accessible during supported offline use. |
| NFR-003 | Temporary unavailability of account or access services must not immediately block a user whose previously verified access is still considered valid. |
| NFR-004 | Offline workspace availability must remain limited by the current access-verification policy. |
| NFR-005 | Operations that require current account, access, or subscription state must clearly indicate when connectivity is required. |

These requirements define product behavior only.

The concrete offline mechanism belongs to:

```text
../../frontend/offline/
../../backend/access/
```

---

## Privacy

| ID | Requirement |
|---|---|
| NFR-006 | Professional workspace information must remain under the user's personal workspace ownership in the current product model. |
| NFR-007 | Normal use of the application must not require professional workspace information to be synchronized to a remote workspace service. |
| NFR-008 | Professional workspace information must remain isolated between different authenticated users. |
| NFR-009 | User-specific visit media must remain isolated between user workspaces. |
| NFR-010 | Logout or access expiration must not remove existing professional workspace information. |

Detailed physical ownership and storage boundaries belong to:

`../../database/`

---

## Security Expectations

| ID | Requirement |
|---|---|
| NFR-011 | Authentication and access state must be handled in a way that prevents unauthorized workspace access. |
| NFR-012 | Sensitive session and access information must not be exposed through normal application data or user-visible workspace content. |
| NFR-013 | The system must support secure session renewal without requiring the user to re-enter credentials for every normal session continuation. |
| NFR-014 | Session renewal must not allow stale or invalid session state to remain trusted indefinitely. |
| NFR-015 | User credentials must not be recoverable from normal stored authentication data. |
| NFR-016 | Secrets that can authorize privileged external or backend operations must not be exposed to the end user. |
| NFR-017 | Integration credentials with privileged access must remain outside normal client-visible configuration. |
| NFR-018 | Production communication that carries authentication, access, or account information must be protected against interception and tampering. |

Concrete security mechanisms belong to:

```text
../../backend/security/
../../operations/security/
../../system/decisions/
```

---

## Access Consistency

| ID | Requirement |
|---|---|
| NFR-019 | Workspace availability must be determined through one consistent access decision model. |
| NFR-020 | Different product screens must not independently produce contradictory access decisions. |
| NFR-021 | Access state must remain consistent across startup, resume, purchase, restore, and access-refresh scenarios. |
| NFR-022 | Reinstalling the application or resetting local workspace data must not incorrectly create a new trial for the same account. |

---

## Data Integrity

| ID | Requirement |
|---|---|
| NFR-023 | Changes to clients, appointments, services, payments, and related visit information must preserve a consistent professional workspace state. |
| NFR-024 | Product updates that change the workspace data model must preserve existing user information whenever the update is considered supported. |
| NFR-025 | Invalid appointment states must be rejected before they become part of the active professional workspace. |
| NFR-026 | Payment actions must not create contradictory payment states for the same professional work. |
| NFR-027 | Information belonging to one user must never appear in another user's active workspace. |

Physical constraints and migration strategy belong to:

`../../database/`

---

## Reliability and Recovery

| ID | Requirement |
|---|---|
| NFR-028 | Application startup must handle missing, invalid, or expired account state without corrupting professional workspace information. |
| NFR-029 | Failure to verify current access must not corrupt or delete existing workspace information. |
| NFR-030 | A failed subscription-state reconciliation must leave the system in a recoverable state. |
| NFR-031 | Unexpected application termination during normal workspace use must not intentionally reset or recreate the user's professional workspace. |
| NFR-032 | Account switching must not leave user-specific reminders or active workspace context attached to the wrong user. |

Detailed recovery behavior belongs to the corresponding technical component.

---

## Performance

| ID | Requirement |
|---|---|
| NFR-033 | Main workspace views should open without unnecessary dependence on remote-service latency. |
| NFR-034 | Calendar and daily-workspace interactions should provide immediate user feedback for normal personal-workspace usage. |
| NFR-035 | Client browsing and search should remain responsive for the expected data volume of an individual specialist. |
| NFR-036 | Access verification should not unnecessarily delay workspace startup when previously verified access can still be trusted. |

The exact measurable thresholds should be defined in technical performance documentation once supported device classes and expected dataset sizes are fixed.

---

## Usability

| ID | Requirement |
|---|---|
| NFR-037 | Primary professional workflows should remain understandable without exposing unnecessary billing, account, or technical complexity. |
| NFR-038 | Subscription information must clearly communicate recurring billing when the selected plan is recurring. |
| NFR-039 | Trial and access messaging must accurately describe the user's current state and must not create misleading expectations. |
| NFR-040 | The user interface must support Russian and English localization. |
| NFR-041 | Visual effects and motion must not prevent completion of normal product workflows. |
| NFR-042 | The product should respect reduced-motion preferences where the target platform exposes them. |

---

## Maintainability

| ID | Requirement |
|---|---|
| NFR-043 | Major product responsibilities should remain separable enough that one area can evolve without requiring unrelated behavior to be rewritten. |
| NFR-044 | Feature-level responsibilities should remain independently understandable and maintainable where practical. |
| NFR-045 | Account and access behavior must remain conceptually separated from professional workspace behavior. |
| NFR-046 | External-system behavior should remain isolated behind explicit integration boundaries. |
| NFR-047 | Important business rules should be testable independently from visual presentation. |

Detailed implementation structure belongs to:

```text
../../backend/
../../frontend/
../../integrations/
../../system/
```

---

## Testability and Verification

| ID | Requirement |
|---|---|
| NFR-048 | Access decisions must be verifiable through automated or repeatable tests. |
| NFR-049 | Authentication and session lifecycle behavior must be verifiable. |
| NFR-050 | Supported workspace-data migrations must be verifiable against existing user data. |
| NFR-051 | Core appointment and payment rules must be covered by repeatable verification. |
| NFR-052 | Production-readiness constraints must be verifiable before release. |

Exact test suites and release checks belong to:

`../../operations/testing/`

---

## Technical Constraints Moved Out of Business

The previous documentation contained several implementation-specific release constraints such as:

```text
specific transport protocol requirements
loopback / emulator endpoint restrictions
development-only mode restrictions
secret placement
release configuration validation
```

These are important, but they are not maintained as canonical business NFRs in this repository structure.

Their canonical location is:

```text
../../operations/release/
../../operations/configuration/
../../backend/security/
../../system/decisions/
```

If one of these technical constraints has a product-level consequence, the consequence may be referenced here without duplicating the implementation rule.

---

## Open Quality Areas

Some NFRs require measurable thresholds before they can be considered complete.

The following areas still need technical calibration:

- supported dataset size for client and appointment search;
- acceptable startup time;
- acceptable calendar interaction latency;
- offline access verification duration;
- supported device baseline;
- recovery expectations after failed local-data migration;
- backup and restore expectations;
- release verification coverage.

These values should be defined after the related technical architecture is documented.

---

## Related Documentation

- [`../scope/scope.md`](../scope/scope.md)
- [`functional-requirements.md`](functional-requirements.md)
- [`business-rules.md`](business-rules.md)
- [`acceptance-criteria.md`](acceptance-criteria.md)
- [`../../database/`](../../database/)
- [`../../backend/`](../../backend/)
- [`../../frontend/`](../../frontend/)
- [`../../operations/`](../../operations/)
- [`../../system/`](../../system/)
