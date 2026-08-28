# Aveli — Acceptance Criteria

<p align="center"><a href="acceptance-criteria.md">English</a> · <a href="acceptance-criteria.ru.md"><b>Русский</b></a></p>

> Observable product conditions для verification requirements/business rules.

## Статус

**Baseline: Stable**

## Authentication

| ID | Acceptance Criterion |
|---|---|
| AC-001 | Новый user может создать account и войти в authenticated state. |
| AC-002 | Existing user может sign in с valid credentials. |
| AC-003 | Invalid credentials не создают authenticated session. |
| AC-004 | Logout завершает active authenticated state. |
| AC-005 | Logout не удаляет professional workspace. |

## Trial and Access

| ID | Acceptance Criterion |
|---|---|
| AC-006 | New account получает один 30-day trial. |
| AC-007 | Logout/sign-in не создаёт новый trial. |
| AC-008 | Reinstall не создаёт новый trial. |
| AC-009 | Valid access source позволяет workspace entry. |
| AC-010 | Без valid access source workspace entry blocked. |
| AC-011 | Valid access unlocks whole workspace. |
| AC-012 | Access expiry preserves workspace information. |
| AC-013 | Restored access возвращает preserved workspace. |

## Subscription

| ID | Acceptance Criterion |
|---|---|
| AC-014 | User может начать monthly/yearly purchase flow. |
| AC-015 | Monthly/yearly subscriptions дают same logical access. |
| AC-016 | Subscription state reconciles с Aveli access. |
| AC-017 | Valid reconciled subscription grants access. |
| AC-018 | Restore valid subscription restores access. |
| AC-019 | Displayed subscription price matches provider/platform. |
| AC-020 | Recurring billing/management location clearly presented. |

## Professional Workspace Data

| ID | Acceptance Criterion |
|---|---|
| AC-021 | Workspace entities usable без continuous backend sync. |
| AC-022 | Created data stays with active user workspace. |
| AC-023 | One user data not shown in another workspace. |
| AC-024 | Visit media not exposed across users. |
| AC-025 | Logout closes context without deleting data. |

## Clients

| ID | Acceptance Criterion |
|---|---|
| AC-026 | Create client and find in directory. |
| AC-027 | Update client and observe changes. |
| AC-028 | Archive/restore client. |
| AC-029 | Open client profile/history. |
| AC-030 | Import permitted device contact without modifying source. |

## Appointments and Visits

| ID | Acceptance Criterion |
|---|---|
| AC-031 | Create appointment satisfying scheduling rules. |
| AC-032 | Created appointment appears Today/Calendar. |
| AC-033 | Create/reschedule violating working hours/conflict rejected. |
| AC-034 | Reschedule appointment and see updated time. |
| AC-035 | Cancel appointment and it is not active scheduled work. |
| AC-036 | No-show distinguishable from cancelled/completed. |
| AC-037 | Complete valid visit. |
| AC-038 | Notes/photos attached to completed visit. |

## Payments

| ID | Acceptance Criterion |
|---|---|
| AC-039 | Record payment for valid payable visit. |
| AC-040 | Completed visit can remain unpaid/partial. |
| AC-041 | Outstanding amount visible until resolved. |
| AC-042 | Period finance reflects workspace payments. |

## Reminders

| ID | Acceptance Criterion |
|---|---|
| AC-043 | Create/use reminder. |
| AC-044 | Open reminder to existing appointment. |
| AC-045 | Logout deactivates reminders. |
| AC-046 | Account switching does not expose reminders. |

## Offline Access

| ID | Acceptance Criterion |
|---|---|
| AC-047 | Verified access permits offline use while policy trusts. |
| AC-048 | Offline access bounded by policy. |
| AC-049 | Expired verification requires renewal. |
| AC-050 | Verification failure does not delete/corrupt workspace. |

## Final Coverage Additions

| ID | Acceptance Criterion |
|---|---|
| AC-056 | Valid stored refresh session restores auth and opens correct local workspace. |
| AC-057 | User can create/update service and use price/duration in planning. |
| AC-058 | Referenced service cannot be removed if it invalidates appointment history. |
| AC-059 | Today shows current daily projection and reflects changes. |
| AC-060 | Calendar changes selected date and shows corresponding appointments. |
| AC-061 | Supported local settings can be changed and remain available according to current persistence. |
| AC-062 | RU/EN localization can be selected without changing workspace ownership. |
| AC-063 | Supported export/import works; automatic conflict merge is not claimed. |
| AC-064 | One appointment cannot have two independent payment records; partial/full stays in one aggregate. |

## Historical Identifier Note

`AC-051`–`AC-055` использовались earlier release-specific acceptance model и intentionally не переиспользуются.

Technical release verification canonical в [`../../system/review/release-readiness.ru.md`](../../system/review/release-readiness.ru.md), [`../../frontend/testing/`](../../frontend/testing/), [`../../frontend/security/`](../../frontend/security/), [`../../backend/security/`](../../backend/security/).

## Remaining Measurement Work

Numeric performance targets и per-release store/device evidence классифицированы в [`../../system/review/open-questions.ru.md`](../../system/review/open-questions.ru.md).

## Related Documentation

- [`business-rules.ru.md`](business-rules.ru.md)
- [`functional-requirements.ru.md`](functional-requirements.ru.md)
- [`non-functional-requirements.ru.md`](non-functional-requirements.ru.md)
- [`../traceability/`](../traceability/)
