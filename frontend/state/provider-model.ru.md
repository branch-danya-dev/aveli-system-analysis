# Модель провайдеров Riverpod

## Контейнер

Aveli вручную создаёт `ProviderContainer` в `main.dart` и передаёт его через `UncontrolledProviderScope`.

## Общие провайдеры

| Провайдер | Роль |
|---|---|
| `localDatabaseManagerProvider` | Открытие и закрытие БД конкретного пользователя. |
| `appDatabaseProvider` | Активная `AppDatabase`. |
| `authControllerProvider` | Сессия и активация рабочего пространства. |
| `accessControllerProvider` | Серверное состояние доступа и его снимок. |
| `accessVerificationPolicyProvider` | Политика доверия при работе без сети. |
| `accessSnapshotStoreProvider` | Безопасное хранение снимка состояния доступа. |
| `networkAvailableProvider` | Поток состояния подключения. |
| `appRouterProvider` | Маршрутизатор и перенаправления. |
| `appBootstrapControllerProvider` | Состояние холодного запуска приложения. |
| `purchaseServiceProvider` | RevenueCat либо заглушка сервиса покупок. |
| `visitReminderSchedulerProvider` | Планировщик системных напоминаний. |
| `scheduleConfigProvider` | Рабочее расписание. |
| `themeManagerProvider` / `appLocaleProvider` | Настройки интерфейса. |

## Время жизни

Большинство провайдеров репозиториев живут на протяжении всей активной сессии.

Подтверждённый пример `autoDispose`:

```text
paywallOfferingsProvider
```

## Направление зависимостей

```text
Представление
    ↓ наблюдение / чтение
Провайдер / контроллер
    ↓
Репозиторий / сценарий
    ↓
Инфраструктура
```

Связывание провайдеров не должно переопределять бизнес-правила; оно лишь объединяет зависимости реализации.
