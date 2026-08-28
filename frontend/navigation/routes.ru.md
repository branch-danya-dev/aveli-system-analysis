# Модель маршрутов

## Маршрутизатор

Каноническая реализация:

```text
lib/app/router.dart
appRouterProvider
GoRouter
initialLocation = /bootstrap
```

## Подтверждённые маршруты

| Путь | Назначение |
|---|---|
| `/bootstrap` | Холодный запуск. |
| `/welcome` | Публичная точка входа. |
| `/register` | Регистрация. |
| `/sign-in` | Вход. |
| `/access-gate` | Доступ заблокирован или требуется проверка через сеть. |
| `/paywall` | Покупка и изменение тарифного плана. |
| `/` | Вкладка рабочего пространства «Сегодня». |
| `/calendar` | Вкладка календаря. |
| `/clients` | Вкладка клиентов. |
| `/clients/:id` | Карточка клиента. |
| `/more` | Вкладка «Ещё». |
| `/more/services` | Услуги. |
| `/more/unpaid` | Неоплаченные суммы. |
| `/more/finances` | Финансы за период. |
| `/more/profile` | Профиль. |
| `/more/profile/subscription` | Сведения о подписке. |
| `/more/appearance` | Оформление. |
| `/more/about` | О приложении. |
| `/appointments/:id` | Карточка записи через корневой навигатор. |

Рабочее пространство использует `StatefulShellRoute` с четырьмя вкладками.

## Глубокие ссылки с параметрами запроса

```text
/?date=YYYY-MM-DD
/calendar?date=YYYY-MM-DD
```

Маршрутизатор обновляет `selectedScheduleDayProvider`.

## Навигация из уведомлений

```text
notification payload
  ↓
pendingReminderAppointmentIdProvider
  ↓
AppShell
  ↓
context.push(/appointments/:id)
```
