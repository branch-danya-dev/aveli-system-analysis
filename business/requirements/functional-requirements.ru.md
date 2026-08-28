# Aveli — Functional Requirements

<p align="center"><a href="functional-requirements.md">English</a> · <a href="functional-requirements.ru.md"><b>Русский</b></a></p>

> Observable product capabilities в current Aveli scope.

## Статус

**Baseline: Stable**

## Account and Authentication

| ID | Требование |
|---|---|
| FR-001 | Позволять новому user создать account. |
| FR-002 | Позволять existing user выполнить sign in. |
| FR-003 | Восстанавливать authenticated session, когда current refresh session можно trusted. |
| FR-004 | Позволять user выполнить logout. |
| FR-005 | Использовать active authenticated user для выбора professional workspace. |

## Trial and Access

| ID | Требование |
|---|---|
| FR-006 | Давать newly created account один 30-day trial. |
| FR-007 | Сохранять original account trial через logout, reinstall и local workspace reset. |
| FR-008 | Определять, может ли current authenticated user открыть workspace. |
| FR-009 | Поддерживать lifetime, manual, subscription и trial access sources. |
| FR-010 | Не позволять workspace entry без valid access source. |
| FR-011 | Применять access к workspace целиком, а не feature-level paywalls. |
| FR-012 | Сохранять professional workspace data при access expiry. |
| FR-013 | Возвращать existing workspace после restore valid access. |

## Subscription

| ID | Требование |
|---|---|
| FR-014 | Запускать supported monthly/yearly subscription purchase flows. |
| FR-015 | Restore existing purchase. |
| FR-016 | Reconcile provider subscription state с Aveli access state. |
| FR-017 | Давать subscription-based workspace access после valid reconciled subscription. |
| FR-018 | Считать monthly/yearly plans одинаковым logical access level. |
| FR-019 | Показывать current platform/provider subscription pricing. |

## Clients

| ID | Требование |
|---|---|
| FR-020 | Создавать client. |
| FR-021 | Обновлять client information. |
| FR-022 | Archive/restore client. |
| FR-023 | Удалять client только если lifecycle rules разрешают и appointment history не требует сохранения client. |
| FR-024 | Browse/search client directory. |
| FR-025 | Открывать client profile и professional history. |
| FR-026 | Создавать/enrich Aveli client из device contact при permission. |

## Services

| ID | Требование |
|---|---|
| FR-027 | Создавать и обновлять services. |
| FR-028 | Хранить service information для planning, включая price и expected duration. |
| FR-029 | Удалять service только если это не инвалидирует existing appointment history; отдельный service-deactivation state отсутствует. |

## Appointments

| ID | Требование |
|---|---|
| FR-030 | Создавать appointment. |
| FR-031 | Требовать client, date/time, service и другое planning information current workflow. |
| FR-032 | Отклонять create/reschedule вне configured working schedule или при conflict с другим active scheduled appointment. |
| FR-033 | Reschedule appointment. |
| FR-034 | Cancel appointment. |
| FR-035 | Mark appointment as no-show. |
| FR-036 | Complete visit. |
| FR-037 | Сохранять visit context: notes/photos. |
| FR-038 | Отражать appointment lifecycle changes в Today/Calendar. |

## Payments

| ID | Требование |
|---|---|
| FR-039 | Записывать payment для work, valid for payment. |
| FR-040 | Позволять completed visit оставаться unpaid/partial. |
| FR-041 | Просматривать outstanding payments. |
| FR-042 | Давать basic period finance из workspace payments. |

## Today and Calendar

| ID | Требование |
|---|---|
| FR-043 | Предоставлять daily workspace view current day. |
| FR-044 | Предоставлять calendar-based appointment navigation. |
| FR-045 | Перемещаться между supported calendar dates. |
| FR-046 | Поддерживать Today/Calendar projections consistent с appointment lifecycle. |

## Reminders

| ID | Требование |
|---|---|
| FR-047 | Использовать reminders для supported appointments. |
| FR-048 | Связывать reminder с appointment context. |
| FR-049 | Деактивировать outgoing-user reminders на logout. |
| FR-050 | Навигировать из valid reminder к related existing appointment. |

## Profile and Settings

| ID | Требование |
|---|---|
| FR-051 | Управлять supported local profile information. |
| FR-052 | Настраивать working schedule. |
| FR-053 | Менять application language. |
| FR-054 | Поддерживать Russian/English localization. |
| FR-055 | Настраивать appearance preferences. |
| FR-056 | Настраивать working currency. |
| FR-057 | Export/import supported workspace data как user-mediated transfer; automatic merge divergent copies вне current stable contract. |

## Professional Workspace Data

| ID | Требование |
|---|---|
| FR-058 | Поддерживать professional workspace без continuous backend sync. |
| FR-059 | Изолировать workspace information между authenticated users. |
| FR-060 | Связывать visit media только с corresponding workspace. |
| FR-061 | Сохранять workspace information на logout. |
| FR-062 | Сохранять workspace information при access expiry. |

## Offline Behavior

| ID | Требование |
|---|---|
| FR-063 | Продолжать normal workspace operations без permanent connectivity. |
| FR-064 | Разрешать temporary offline access по previously verified access, если policy permits. |
| FR-065 | Требовать renewed verification, когда offline policy больше не trusted previous verification. |
| FR-066 | Требовать connectivity для current account/access/subscription verification. |

## Requirement Boundary

Functional requirements определяют product behavior, а не framework/schema choices.

Lifecycle clarifications canonical в [`business-rules.ru.md`](business-rules.ru.md). Low-level interval semantics, provider/store config, numeric performance targets и future automatic workspace merge здесь не придумываются.

## Related Documentation

- [`business-rules.ru.md`](business-rules.ru.md)
- [`non-functional-requirements.ru.md`](non-functional-requirements.ru.md)
- [`acceptance-criteria.ru.md`](acceptance-criteria.ru.md)
- [`../traceability/`](../traceability/)
- [`../../system/review/open-questions.ru.md`](../../system/review/open-questions.ru.md)
