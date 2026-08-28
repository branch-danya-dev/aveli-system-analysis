# Карта возможностей рабочего пространства

## Клиенты

```text
Представление
→ ClientsScreen / ClientDetailsScreen / редактор / импорт контактов
Состояние
→ clients_providers.dart
Домен
→ ClientRepository + сценарии создания / обновления / удаления / архивации / импорта
Доступ к данным
→ client_repository_impl.dart / client_directory_repository_impl.dart
Хранение
→ таблица clients через Drift
```

## Услуги

```text
Представление → ServicesScreen / редактор
Состояние     → services_providers.dart
Домен         → ServiceRepository / SaveService / DeleteService
Доступ к данным → service_repository_impl.dart
Хранение      → таблица services через Drift
```

## Записи / визиты

```text
Представление → создание / детали / завершение / отмена
Состояние     → appointments_providers.dart
Домен         → сценарии записей и визитов
Доступ к данным → appointment_repository_impl / visit_repository_impl
Хранение      → appointments + visit_notes + visit_photos
Связи         → напоминания + оплаты + клиенты + услуги
```

## Сегодня / календарь

Реактивные представления используют обновления таблиц Drift.

```text
TodayScreen / CalendarScreen / DayBoard
→ провайдеры состояния
→ репозитории
→ ScheduleDayProjection / объединения Drift
```

## Оплаты / задолженности

```text
OutstandingPaymentsScreen
PeriodFinanceScreen
AcceptPaymentSheet
→ провайдеры и сценарии оплат
→ репозитории оплат
→ payments + appointments.payment_status
```

## Настройки / профиль

Профиль и настройки — локальные данные приложения, а не зеркало серверной сущности `users`.

Хранение: `app_settings`.

Поведение клиента включает оформление, локальный профиль, признаки публичного размещения, импорт и экспорт, а также конвертацию валют.

## Расписание

`ScheduleConfig` хранит минуты начала и окончания рабочего времени для создания записей, проверки временных слотов и построения дневного расписания.
