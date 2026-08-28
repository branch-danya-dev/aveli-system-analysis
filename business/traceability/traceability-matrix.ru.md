# Aveli — Traceability Matrix

<p align="center">
  <a href="traceability-matrix.md">English</a> ·
  <a href="traceability-matrix.ru.md"><b>Русский</b></a>
</p>

> Связывает бизнес-правила, требования, критерии приемки, требования к качеству и технический ownership Aveli.

---

## Назначение

Traceability в Aveli шире традиционной цепочки:

```text
Requirement
    ↓
Test
```

Целевая модель:

```text
Business Intent
    ↓
Business Rule
    ↓
Functional Requirement
    ↓
Quality Constraint
    ↓
Acceptance Criterion
    ↓
Technical Ownership
```

Матрица не переопределяет эти артефакты.

Она только фиксирует связи между ними.

---

## Статус покрытия

Используются три состояния:

| Статус | Значение |
|---|---|
| **Covered** | Для значимого поведения существует осмысленная цепочка от требования или правила до проверки и технического ownership. |
| **Partial** | Поведение определено, но отсутствует хотя бы одна важная связь, измеримый критерий или финальное правило. |
| **Open** | Продуктовое правило еще недостаточно определено для стабильного trace. |

Отсутствующая связь намеренно остается видимой.

Traceability должна показывать неопределенность, а не скрывать ее.

---

## Core Product Traceability

| Область | Business Rules | Functional Requirements | Relevant NFR | Acceptance Criteria | Technical Ownership | Статус |
|---|---|---|---|---|---|---|
| Authentication | BR-043–BR-047 | FR-001–FR-005 | NFR-011, NFR-013–NFR-015, NFR-049 | AC-001–AC-005 | `backend/auth/`, `frontend/bootstrap/` | **Partial** |
| Access decision | BR-001–BR-007 | FR-008–FR-011 | NFR-019–NFR-021 | AC-009–AC-011 | `backend/access/`, `frontend/bootstrap/` | **Covered** |
| Trial lifecycle | BR-008–BR-013 | FR-006–FR-007 | NFR-022 | AC-006–AC-008 | `backend/access/`, `database/server/` | **Covered** |
| Access expiration & restoration | BR-025–BR-026 | FR-012–FR-013, FR-061–FR-062 | NFR-010, NFR-028–NFR-029 | AC-012–AC-013, AC-025, AC-050 | `backend/access/`, `frontend/storage/`, `database/` | **Covered** |
| Subscription | BR-014–BR-019 | FR-014–FR-019 | NFR-021, NFR-030, NFR-038–NFR-039 | AC-014–AC-020 | `integrations/`, `backend/billing/`, `backend/access/`, `frontend/` | **Covered** |
| Client management | BR-039–BR-040, BR-042 | FR-020–FR-025 | NFR-023, NFR-027, NFR-035 | AC-026–AC-029 | `frontend/workspace/clients/`, `database/local/` | **Partial** |
| Device contact import | BR-041 | FR-026 | NFR-006–NFR-009 | AC-030 | `integrations/device-contacts/`, `frontend/workspace/clients/` | **Covered** |
| Services | — | FR-027–FR-029 | NFR-023 | — | `frontend/workspace/services/`, `database/local/` | **Open** |
| Appointments & visits | BR-027–BR-033 | FR-030–FR-038 | NFR-023, NFR-025, NFR-034 | AC-031–AC-038 | `frontend/workspace/appointments/`, `database/local/` | **Partial** |
| Payments | BR-034–BR-038 | FR-039–FR-042 | NFR-023, NFR-026 | AC-039–AC-042 | `frontend/workspace/payments/`, `database/local/` | **Partial** |
| Today & Calendar | — | FR-043–FR-046 | NFR-033–NFR-034 | AC-032 косвенно | `frontend/workspace/today/`, `frontend/workspace/calendar/` | **Partial** |
| Reminders | BR-053–BR-056 | FR-047–FR-050 | NFR-032 | AC-043–AC-046 | `frontend/notifications/` | **Covered** |
| Profile & settings | — | FR-051–FR-057 | NFR-040–NFR-042 | — | `frontend/workspace/settings/`, `frontend/localization/`, `database/local/` | **Partial** |
| Workspace ownership & isolation | BR-020–BR-026 | FR-058–FR-062 | NFR-006–NFR-010, NFR-023–NFR-024, NFR-027 | AC-021–AC-025 | `database/`, `frontend/storage/`, `backend/auth/` | **Covered** |
| Offline workspace | BR-048–BR-052 | FR-063–FR-066 | NFR-001–NFR-005, NFR-028–NFR-029, NFR-036 | AC-047–AC-050 | `frontend/offline/`, `backend/access/` | **Partial** |

---

## Детальный Access Trace

Access — одна из самых cross-cutting областей продукта и хороший пример целевой модели traceability.

```text
BR-001..BR-007
Workspace доступен только при наличии действующего источника доступа
        ↓
FR-008..FR-011
Продукт должен определить и применить workspace-level access
        ↓
NFR-019..NFR-021
Решение должно быть согласованным во всех состояниях продукта
        ↓
AC-009..AC-011
Разрешенный пользователь входит в workspace; запрещенный попадает в Access Gate
        ↓
backend/access/
frontend/bootstrap/
```

Истечение доступа продолжает цепочку:

```text
BR-026
Истечение доступа не должно удалять профессиональную информацию
        ↓
FR-012 / FR-062
Access expiration сохраняет существующие профессиональные данные
        ↓
NFR-010 / NFR-029
Потеря или ошибка проверки доступа не должна уничтожать workspace
        ↓
AC-012 / AC-050
Существующая информация сохраняется
        ↓
backend/access/
frontend/storage/
database/
```

---

## Детальный User Isolation Trace

Изоляция пользователей пересекает authentication, ownership данных, local storage и reminders.

```text
BR-023 / BR-024 / BR-047 / BR-054
Workspace и пользовательский контекст должны быть изолированы
        ↓
FR-005 / FR-059 / FR-060 / FR-049
Активный аккаунт определяет ownership workspace и user-specific context
        ↓
NFR-008 / NFR-009 / NFR-027 / NFR-032
Информация не должна утекать между активными пользовательскими контекстами
        ↓
AC-023 / AC-024 / AC-045 / AC-046
Смена аккаунта и logout не раскрывают данные или reminders другого пользователя
        ↓
database/
frontend/storage/
frontend/notifications/
backend/auth/
```

Это пример одного бизнес-инварианта, влияющего сразу на несколько технических компонентов.

---

## Non-Functional Traceability

Не каждый NFR естественно связывается с одним business rule или acceptance criterion.

Quality requirements также связываются с технической областью, которая должна сделать их измеримыми.

| Quality Area | NFR | Current Verification | Technical Ownership | Статус |
|---|---|---|---|---|
| Offline availability | NFR-001–NFR-005 | AC-047–AC-050 + workspace scenarios | `frontend/offline/`, `backend/access/` | **Partial** |
| Privacy & workspace ownership | NFR-006–NFR-010 | AC-021–AC-025 | `database/`, `frontend/storage/` | **Covered** |
| Security | NFR-011–NFR-018 | Authentication/product scenarios + будущие technical security checks | `backend/security/`, `operations/security/` | **Partial** |
| Access consistency | NFR-019–NFR-022 | AC-006–AC-013, AC-016–AC-018 | `backend/access/`, `frontend/bootstrap/` | **Covered** |
| Data integrity | NFR-023–NFR-027 | Client, appointment, payment и isolation acceptance scenarios | `database/`, соответствующие workspace modules | **Partial** |
| Reliability & recovery | NFR-028–NFR-032 | AC-023–AC-025, AC-045–AC-050 | соответствующие technical components | **Partial** |
| Performance | NFR-033–NFR-036 | Измеримые thresholds еще не зафиксированы | `frontend/`, `operations/testing/` | **Open** |
| Usability | NFR-037–NFR-042 | Subscription/access scenarios; localization и motion verification ожидаются | `frontend/` | **Partial** |
| Maintainability | NFR-043–NFR-047 | Architecture review и automated checks еще не определены | `system/`, component architecture | **Open** |
| Testability | NFR-048–NFR-052 | Technical test strategy еще не описана | `operations/testing/` | **Open** |

---

## Известные Traceability Gaps

Текущая матрица намеренно показывает несколько незакрытых областей.

### Authentication Session Restoration

`FR-003` определяет восстановление сессии, но текущий acceptance document не содержит отдельного критерия для успешного session restoration.

После финализации session lifecycle следует добавить dedicated criterion.

### Services

`FR-027`–`FR-029` описывают управление услугами.

Dedicated business rules и acceptance criteria для service lifecycle еще не определены.

### Appointment Conflict Model

Основной appointment flow покрыт, но точная модель конфликтов слотов остается открытой.

`BR-029`, `BR-030`, `FR-032` и `AC-033` нельзя сделать полностью точными до финализации scheduling rules.

### Client Lifecycle

Archive/restore описан, но финальные условия delete еще открыты.

### Payment Lifecycle

Базовое поведение оплат покрыто, но duplicate payment handling и полный payment-state lifecycle еще не определены.

### Today и Calendar

`FR-043`–`FR-046` определяют основные views, но dedicated acceptance criteria для их поведения неполны.

### Profile, Settings, Export и Import

У `FR-051`–`FR-057` сейчас нет отдельных acceptance criteria.

Для export/import также нужны явные conflict и compatibility rules.

### Performance

`NFR-033`–`NFR-036` намеренно остаются неизмеримыми до фиксации поддерживаемых классов устройств, ожидаемых объемов данных и целевых thresholds.

### Offline Verification Duration

Продуктовое поведение определено, но точная длительность и verification policy остаются техническими/open решениями.

---

## Историческая миграция

Предыдущая traceability model содержала раздел `Release Safety`, связывавший:

```text
BR-057..BR-061
NFR-053..NFR-055
AC-051..AC-055
```

Эти элементы описывали implementation-specific release и security constraints.

По текущим правилам репозитория они больше не являются canonical business artifacts.

Их ответственность переносится в:

```text
operations/release/
operations/testing/
operations/configuration/
backend/security/
system/decisions/
```

`AC-051`–`AC-055` намеренно остаются неиспользованными в business acceptance document, чтобы исторические ссылки не были незаметно переназначены.

---

## Направление Traceability

Trace можно читать в обе стороны.

### От Business к Implementation

```text
Зачем существует это поведение?
    ↓
Какое правило его определяет?
    ↓
Какое требование его выражает?
    ↓
Как оно проверяется?
    ↓
Какой компонент отвечает за реализацию?
```

### От Technology к Business

```text
Зачем существует этот компонент?
    ↓
Какое требование он поддерживает?
    ↓
Какое бизнес-правило или продуктовое поведение от него зависит?
```

Второе направление будет становиться особенно важным по мере наполнения технической документации.

Оно позволит обосновывать архитектуру и технологии продуктовыми потребностями, а не оставлять их изолированными implementation facts.

---

## Future Machine-Readable Traceability

Human-readable матрица сейчас является основной traceability view.

Методология изначально допускает machine-readable связи:

```yaml
id: TRACE-ACCESS-001

business_rules:
  - BR-001
  - BR-002

functional_requirements:
  - FR-008
  - FR-009

non_functional_requirements:
  - NFR-019

acceptance_criteria:
  - AC-009

technical_owners:
  - backend/access
  - frontend/bootstrap

status: covered
```

В будущем это позволит строить automated repository checks, dependency graphs, coverage reports и AI-assisted analysis.

---

## Связанная документация

- [`../requirements/business-rules.ru.md`](../requirements/business-rules.ru.md)
- [`../requirements/functional-requirements.ru.md`](../requirements/functional-requirements.ru.md)
- [`../requirements/non-functional-requirements.ru.md`](../requirements/non-functional-requirements.ru.md)
- [`../requirements/acceptance-criteria.ru.md`](../requirements/acceptance-criteria.ru.md)
- [`../processes/`](../processes/)
- [`../../rules.ru.md`](../../rules.ru.md)
