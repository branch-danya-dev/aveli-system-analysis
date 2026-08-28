# Aveli — Матрица трассируемости

<p align="center"><a href="traceability-matrix.md">Английский</a> · <a href="traceability-matrix.ru.md"><b>Русский</b></a></p>

> Связывает бизнес-правила, требования, критерии приёмки, требования к качеству и реальных технических владельцев.

## Статус

**Стабильный базовый уровень**

| Область | Бизнес-правила | FR | NFR | Критерии приёмки | Технический владелец | Статус |
|---|---|---|---|---|---|---|
| Аутентификация | BR-043–047 | FR-001–005 | NFR-011, 013–015, 049 | AC-001–005, AC-056 | `backend/auth/`, `frontend/auth/`, `frontend/bootstrap/` | Покрыто |
| Решение о доступе | BR-001–007 | FR-008–011 | NFR-019–021 | AC-009–011 | `backend/access/`, `frontend/access/` | Покрыто |
| Жизненный цикл пробного периода | BR-008–013 | FR-006–007 | NFR-022 | AC-006–008 | `backend/access/`, `database/server/` | Покрыто |
| Окончание и восстановление доступа | BR-025–026 | FR-012–013, 061–062 | NFR-010, 028–029 | AC-012–013, 025, 050 | `backend/access/`, `frontend/access/`, `frontend/storage/`, `database/` | Покрыто |
| Подписка | BR-014–019 | FR-014–019 | NFR-021, 030, 038–039 | AC-014–020 | `frontend/billing/`, `backend/billing/`, `integrations/revenuecat/` | Покрыто |
| Клиенты | BR-039–042, 062–063 | FR-020–026 | NFR-023, 027, 035 | AC-026–030 | `frontend/workspace/feature-map.md`, `database/local/entities/clients.md`, `integrations/device-contacts/` | Покрыто |
| Услуги | BR-064–065 | FR-027–029 | NFR-023 | AC-057–058 | `frontend/workspace/feature-map.md`, `database/local/entities/services.md` | Покрыто |
| Записи / визиты | BR-027–033, 066–068 | FR-030–038 | NFR-023, 025, 034 | AC-031–038 | `frontend/workspace/feature-map.md`, `database/local/entities/appointments.md`, `frontend/errors/` | Покрыто |
| Оплаты | BR-034–038, 069–071 | FR-039–042 | NFR-023, 026 | AC-039–042, AC-064 | `frontend/workspace/feature-map.md`, `database/local/entities/payments.md` | Покрыто |
| Сегодня / Календарь | BR-029–031, 066–068 | FR-043–046 | NFR-033–034 | AC-032, 059–060 | `frontend/workspace/feature-map.md`, `frontend/navigation/` | Покрыто |
| Напоминания | BR-053–056 | FR-047–050 | NFR-032 | AC-043–046 | `frontend/notifications/`, `integrations/device-notifications/` | Покрыто |
| Профиль / настройки / перенос данных | BR-072 | FR-051–057 | NFR-040–042 | AC-061–063 | `frontend/workspace/feature-map.md`, `database/local/entities/app_settings.md`, `integrations/device-handoff/` | Покрыто в рамках текущего контракта |
| Изоляция рабочего пространства | BR-020–026, 047 | FR-058–062 | NFR-006–010, 027 | AC-021–025 | `database/`, `frontend/storage/`, `backend/auth/` | Покрыто |
| Офлайн-работа | BR-048–052 | FR-063–066 | NFR-001–005, 028–029, 036 | AC-047–050 | `frontend/offline/`, `frontend/access/`, `backend/access/` | Покрыто |

## Проверка нефункциональных требований

| Область качества | Где проверяется |
|---|---|
| Офлайн-режим и согласованность доступа | `frontend/access/`, `frontend/offline/`, `backend/access/`, `system/review/failure-scenarios.ru.md` |
| Конфиденциальность и изоляция | `database/`, `frontend/storage/`, `system/trust/` |
| Безопасность | `frontend/security/`, `backend/security/`, `integrations/`, `system/trust/` |
| Целостность данных | `database/`, `frontend/testing/`, документы владельцев рабочих сценариев |
| Надёжность и восстановление | `system/review/failure-scenarios.ru.md`, тесты компонентов |
| Производительность | будущие измеримые показатели; см. реестр закрытия вопросов |
| Готовность к релизу | `system/review/release-readiness.ru.md` |
| Тестируемость | `frontend/testing/`, серверные тесты и подтверждения интеграций |

Отдельной области `operations/` в текущей базовой версии Aveli нет.

## Результат покрытия

Все текущие функциональные области продукта имеют цепочку:

```text
бизнес-правило / требование
        ↓
критерий приёмки
        ↓
технический владелец
```

Оставшиеся вопросы не скрываются как долг трассируемости. Они явно классифицированы как:

```text
будущее продуктовое решение
внешнее подтверждение провайдера
проверка конкретной платформы
будущий измеримый норматив
изменение архитектурной границы
```

См. [`../../system/review/open-questions.ru.md`](../../system/review/open-questions.ru.md).

## Связанные документы

- [`../requirements/business-rules.ru.md`](../requirements/business-rules.ru.md)
- [`../requirements/functional-requirements.ru.md`](../requirements/functional-requirements.ru.md)
- [`../requirements/non-functional-requirements.ru.md`](../requirements/non-functional-requirements.ru.md)
- [`../requirements/acceptance-criteria.ru.md`](../requirements/acceptance-criteria.ru.md)
- [`../../system/`](../../system/)
