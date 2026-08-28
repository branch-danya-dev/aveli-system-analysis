# Aveli — Business Rules

<p align="center"><a href="business-rules.md">English</a> · <a href="business-rules.ru.md"><b>Русский</b></a></p>

> Product-level invariants и lifecycle constraints независимо от implementation.

## Статус

**Baseline: Stable**

`BR-057`–`BR-061` intentionally retired historical identifiers из удалённой release-rule model и не переиспользуются.

## Access Rules

| ID | Правило |
|---|---|
| BR-001 | Пользователь может открыть workspace только при наличии хотя бы одного valid access source. |
| BR-002 | Access sources оцениваются в priority Lifetime → Manual Grant → Active Subscription → Active Trial → None. |
| BR-003 | Lifetime access имеет приоритет над остальными access sources. |
| BR-004 | Manual access имеет приоритет над subscription и trial. |
| BR-005 | Active subscription имеет приоритет над active trial. |
| BR-006 | Если valid access source отсутствует, workspace недоступен. |
| BR-007 | Workspace access выдаётся или блокируется целиком; current product не использует independent feature-level premium gates. |

## Trial Rules

| ID | Правило |
|---|---|
| BR-008 | Новый account получает один 30-day registration trial. |
| BR-009 | Trial state принадлежит account, а не local installation state. |
| BR-010 | Logout не перезапускает и не продлевает trial. |
| BR-011 | Reinstall не создаёт новый trial для того же account. |
| BR-012 | Удаление local workspace data не создаёт новый trial. |
| BR-013 | Current product не комбинирует Aveli registration trial со вторым store-managed free trial. |

## Subscription Rules

| ID | Правило |
|---|---|
| BR-014 | Monthly и yearly subscriptions дают одинаковый logical workspace access level. |
| BR-015 | Показанная subscription price берётся из platform/provider pricing, а не из independently maintained static value. |
| BR-016 | Recurring store subscription показывается пользователю как recurring. |
| BR-017 | Subscription management/cancellation выполняется через соответствующую mobile platform. |
| BR-018 | Успешный client purchase flow не обходит common Aveli access decision. |
| BR-019 | Restore valid subscription восстанавливает subscription-based workspace access после reconciliation. |

## Workspace Data Ownership

| ID | Правило |
|---|---|
| BR-020 | Clients, appointments, services, payments, visit notes и visit photos принадлежат professional workspace domain. |
| BR-021 | Professional workspace information не синхронизируется между devices в current product model. |
| BR-022 | Account/access responsibilities отделены от professional workspace ownership. |
| BR-023 | Каждый user имеет isolated professional workspace. |
| BR-024 | User-specific workspace materials не должны быть exposed между разными workspaces. |
| BR-025 | Logout не удаляет persistent professional workspace information. |
| BR-026 | Expiration trial/subscription/другого access source не удаляет и не меняет existing professional workspace information. |

## Appointment Rules

| ID | Правило |
|---|---|
| BR-027 | Appointment принадлежит valid client. |
| BR-028 | Appointment ссылается на valid service, когда service selection требуется workflow. |
| BR-029 | Appointment date/time соответствует configured scheduling rules. |
| BR-030 | Conflicting appointments отклоняются согласно current slot-availability model. |
| BR-031 | Cancelled appointment не считается active scheduled visit. |
| BR-032 | No-show остаётся отличимым от cancelled/completed visit. |
| BR-033 | Completed visit может сохранять notes, photos и payment information. |

## Payment Rules

| ID | Правило |
|---|---|
| BR-034 | Payment записывается только для work, valid for payment в current visit lifecycle. |
| BR-035 | Completed visit может оставаться unpaid. |
| BR-036 | Outstanding payments остаются visible до resolution. |
| BR-037 | Payment state согласован с related visit state. |
| BR-038 | Financial summaries основаны на payment information professional workspace. |

## Client Rules

| ID | Правило |
|---|---|
| BR-039 | Client records принадлежат только active user workspace. |
| BR-040 | Archive client сохраняет historical information. |
| BR-041 | Import device contact создаёт/enriches Aveli client и не меняет original contact. |
| BR-042 | Client history derived из professional activity данного client. |

## Session and Account Switching

| ID | Правило |
|---|---|
| BR-043 | Logout завершает active authenticated session. |
| BR-044 | После logout previous workspace inactive до повторной authentication. |
| BR-045 | User-specific reminders outgoing account деактивируются на logout. |
| BR-046 | Logout не удаляет persistent workspace outgoing user. |
| BR-047 | После activation другого account данные previous user не exposed в новом workspace. |

## Offline Access

| ID | Правило |
|---|---|
| BR-048 | Previously verified access state может временно разрешать offline workspace access. |
| BR-049 | Offline access valid только в current verification policy. |
| BR-050 | Когда offline verification недостаточна, требуется renewed verification. |
| BR-051 | Normal professional workspace operations не требуют continuous network connectivity. |
| BR-052 | Operations для current account/access/subscription verification требуют connectivity. |

## Reminder Rules

| ID | Правило |
|---|---|
| BR-053 | Appointment reminders принадлежат appointments active user workspace. |
| BR-054 | Reminders не должны expose appointment information другого user после account switching. |
| BR-055 | Logout деактивирует reminders outgoing user. |
| BR-056 | Opening valid reminder ведёт к related appointment, если он существует и доступен. |

## Final Lifecycle Clarifications

| ID | Правило |
|---|---|
| BR-062 | Permanent client deletion разрешён только если appointment history не references client; иначе используется archive. |
| BR-063 | Archive — normal history-preserving способ убрать client из active directory; restore возвращает его. |
| BR-064 | Service, referenced existing appointments, должен сохраняться, чтобы не инвалидировать historical appointment meaning. |
| BR-065 | Current product не определяет отдельный service-deactivation state; service либо available, либо safely deleted по references. |
| BR-066 | Appointment start/end должны помещаться в configured working schedule current workspace. |
| BR-067 | Create/reschedule appointment отклоняется при conflict с другим active scheduled appointment. |
| BR-068 | Cancelled appointments не являются active scheduled work и не участвуют как active conflicts. |
| BR-069 | Current payment model поддерживает максимум один aggregate payment record на appointment. |
| BR-070 | Appointment payment может проходить unpaid → partial → paid внутри одного aggregate payment record. |
| BR-071 | Второй independent payment record для того же appointment не является valid current product state. |
| BR-072 | Export/import — user-mediated transfer capability; automatic merge divergent workspace copies не входит в current stable product contract. |

## Technical Ownership

- access resolution → [`../../backend/access/`](../../backend/access/)
- local persistence → [`../../database/local/`](../../database/local/)
- client behavior → [`../../frontend/`](../../frontend/)
- external billing → [`../../integrations/revenuecat/`](../../integrations/revenuecat/)
- cross-system trust/failure/release → [`../../system/`](../../system/)

## Related Documentation

- [`functional-requirements.ru.md`](functional-requirements.ru.md)
- [`non-functional-requirements.ru.md`](non-functional-requirements.ru.md)
- [`acceptance-criteria.ru.md`](acceptance-criteria.ru.md)
- [`../traceability/`](../traceability/)
