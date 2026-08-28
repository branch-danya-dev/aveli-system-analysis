# Aveli — Non-Functional Requirements

<p align="center"><a href="non-functional-requirements.md">English</a> · <a href="non-functional-requirements.ru.md"><b>Русский</b></a></p>

> Product-level quality expectations и constraints.

## Статус

**Baseline: Stable**

## Availability and Offline

| ID | Требование |
|---|---|
| NFR-001 | Core workspace operations остаются usable без continuous network connectivity. |
| NFR-002 | Previously available workspace data доступны в supported offline use. |
| NFR-003 | Temporary account/access-service unavailability не блокирует user, пока previously verified access trusted. |
| NFR-004 | Offline workspace availability ограничена current access-verification policy. |
| NFR-005 | Operations для current account/access/subscription state явно требуют connectivity. |

## Privacy

| ID | Требование |
|---|---|
| NFR-006 | Professional workspace information остаётся under personal workspace ownership. |
| NFR-007 | Normal use не требует sync professional workspace в remote workspace service. |
| NFR-008 | Workspace information isolated между authenticated users. |
| NFR-009 | User-specific visit media isolated между workspaces. |
| NFR-010 | Logout/access expiry не удаляет workspace information. |

## Security Expectations

| ID | Требование |
|---|---|
| NFR-011 | Authentication/access handling предотвращает unauthorized workspace access. |
| NFR-012 | Sensitive session/access state не exposed через workspace data/UI. |
| NFR-013 | System поддерживает secure session renewal без credential re-entry каждый раз. |
| NFR-014 | Stale/invalid session state не trusted indefinitely. |
| NFR-015 | User passwords не recoverable из stored auth data. |
| NFR-016 | Privileged backend/external secrets не exposed end user. |
| NFR-017 | Privileged integration credentials остаются вне client-visible config. |
| NFR-018 | Production account/access communication защищена in transit. |

## Access Consistency

| ID | Требование |
|---|---|
| NFR-019 | Workspace availability использует один consistent access decision model. |
| NFR-020 | Different screens не produce contradictory access decisions. |
| NFR-021 | Access state consistent across startup/resume/purchase/restore/refresh. |
| NFR-022 | Reinstall/local reset не создаёт новый trial. |

## Data Integrity

| ID | Требование |
|---|---|
| NFR-023 | Workspace changes сохраняют consistent professional data state. |
| NFR-024 | Supported data-model updates preserve existing user data. |
| NFR-025 | Invalid appointment states rejected до active workspace state. |
| NFR-026 | Payment actions не создают contradictory payment states для appointment. |
| NFR-027 | Data одного user не появляются в active workspace другого. |

## Reliability and Recovery

| ID | Требование |
|---|---|
| NFR-028 | Startup handles invalid account state без corruption workspace. |
| NFR-029 | Failed access verification не corrupt/delete workspace. |
| NFR-030 | Failed subscription reconciliation оставляет recoverable state. |
| NFR-031 | Unexpected termination не intentionally reset/recreate workspace. |
| NFR-032 | Account switching не attach reminders/workspace context wrong user. |

## Performance

| ID | Требование |
|---|---|
| NFR-033 | Main workspace views избегают unnecessary remote latency dependency. |
| NFR-034 | Calendar/daily interactions дают immediate feedback. |
| NFR-035 | Client browse/search responsive для expected individual-specialist volume. |
| NFR-036 | Access verification не unnecessarily delays startup при trusted cache. |

## Usability

| ID | Требование |
|---|---|
| NFR-037 | Primary workflows понятны без unnecessary complexity. |
| NFR-038 | Recurring subscription clearly presented. |
| NFR-039 | Trial/access messaging accurately reflects state. |
| NFR-040 | UI поддерживает Russian/English localization. |
| NFR-041 | Visual effects/motion не block workflows. |
| NFR-042 | Product respects reduced-motion preferences where supported. |

## Maintainability

| ID | Требование |
|---|---|
| NFR-043 | Major responsibilities остаются separable для independent evolution. |
| NFR-044 | Feature responsibilities independently understandable/maintainable. |
| NFR-045 | Account/access conceptually separate от workspace. |
| NFR-046 | External-system behavior behind explicit integration boundaries. |
| NFR-047 | Important business rules testable независимо от presentation. |

## Testability and Verification

| ID | Требование |
|---|---|
| NFR-048 | Access decisions verifiable repeatable tests. |
| NFR-049 | Authentication/session lifecycle verifiable. |
| NFR-050 | Workspace migrations verifiable against existing user data. |
| NFR-051 | Core appointment/payment rules have repeatable verification. |
| NFR-052 | Production-readiness constraints verifiable before release. |

## Verification Ownership

Technical verification принадлежит existing owners:

```text
frontend/testing/
frontend/security/
backend/security/
integrations/
system/trust/
system/review/failure-scenarios.ru.md
system/review/release-readiness.ru.md
```

Standalone `operations/` perspective в current Aveli baseline отсутствует.

## Future Measurable Calibration

Baseline не invent numeric targets. Future calibration может определить supported devices, expected data volume, startup/calendar/search latency, backup expectations и per-release test evidence.

См. [`../../system/review/open-questions.ru.md`](../../system/review/open-questions.ru.md).

## Related Documentation

- [`functional-requirements.ru.md`](functional-requirements.ru.md)
- [`acceptance-criteria.ru.md`](acceptance-criteria.ru.md)
- [`../../system/trust/`](../../system/trust/)
