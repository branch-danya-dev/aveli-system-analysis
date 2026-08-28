# Сценарий запуска Aveli

## Инициализация до построения интерфейса

Подтверждённый порядок:

```text
main()
 ↓
WidgetsFlutterBinding.ensureInitialized()
 ↓
ReleaseConfigGate.assertForCurrentBuild()
 ↓
настроить системный интерфейс в режиме edge-to-edge
 ↓
инициализировать форматирование дат ru/en
 ↓
ProviderContainer()
 ↓
восстановить локаль гостевого режима
 ↓
runApp(UncontrolledProviderScope → AveliApp)
```

Drift здесь **не** открывается.

Настройка RevenueCat и инициализация уведомлений выполняются лениво.

## Начальный маршрут

`GoRouter.initialLocation`:

```text
/bootstrap
```

Далее `AppBootstrapController` определяет состояние приложения.

## Инициализация после входа

```text
восстановить сессию по токену обновления
        ↓
открыть aveli_<userId>.sqlite
        ↓
RevenueCat logIn(userId)
        ↓
восстановить тему из Drift
        ↓
автономный режим?
   /               \
 да                 нет
  ↓                  ↓
готово       обновить /v1/access
                     ↓
             необязательное тестовое заполнение
                     ↓
              дождаться состояния доступа
                     ↓
             определить результат проверки доступа
```

## Результаты начальной инициализации

- `BootstrapNeedsWelcome`
- `BootstrapReady`
- `BootstrapAccessRequired`
- `BootstrapNeedsVerification`
- `BootstrapFailure`

Экран начальной инициализации преобразует результат в переход маршрутизатора.

## Ленивая инициализация сервисов

- `Purchases.configure` → вызывается лениво в `RevenueCatPurchaseService.ensureConfigured()`
- инициализация уведомлений → выполняется лениво в `LocalVisitReminderScheduler.ensureInitialized()`
- база данных → открывается после аутентификации при активации рабочего пространства, а не до `runApp`

## Примечание о проверке

Максимальное время ожидания инициализации задаётся `BootstrapTiming.maxInitialization`.
