# Структура фронтенд-приложения

## Проверенная структура исходного кода

```text
lib/
├── app/
├── core/
│   ├── config/
│   ├── database/
│   ├── design/
│   ├── design_system/
│   ├── domain/
│   ├── errors/
│   ├── l10n/
│   ├── providers/
│   └── widgets/
├── features/
│   ├── appointments/
│   ├── auth/
│   ├── bootstrap/
│   ├── calendar/
│   ├── clients/
│   ├── legal/
│   ├── more/
│   ├── payments/
│   ├── reminders/
│   ├── services/
│   ├── settings/
│   ├── subscription/
│   └── today/
└── l10n/
```

## Архитектурный стиль

Текущая реализация использует **гибридный подход с организацией по возможностям** (`feature-first`).

Возможности приложения обычно организуют собственные:

```text
представление
домен
доступ к данным
```

Общая инфраструктура находится в `core/`.

Тонкий слой `app/` отвечает за общую композицию приложения: маршрутизатор, оболочку, вспомогательную логику запуска и привязку к жизненному циклу.

## Направление зависимостей

Типичная цепочка:

```text
Экран
  ↓
Провайдер / контроллер
  ↓
Доменный сценарий / интерфейс репозитория
  ↓
Реализация репозитория
  ↓
Drift / HTTP / служба устройства
```

Не каждая возможность использует все уровни.

## Характерные цепочки

### Сегодня

```text
TodayScreen
  → todayOverviewProvider
  → todayRepositoryProvider
  → TodayRepositoryImpl
  → репозитории функциональных областей / AppDatabase
```

### Регистрация

```text
RegisterScreen
  → AuthController.register
  → AuthRepositoryImpl
  → AuthRemoteDataSource
  → SecureTokenStorage
  → LocalDatabaseManager
  → PurchaseService.logIn
```

### Покупка

```text
Экран подписки
  → AccessController.purchasePackage
  → RevenueCatPurchaseService.purchase
  → AccessController.syncBilling
  → серверный AccessStatusView
  → защищённый снимок состояния
```

## Каноническое владение

Документ описывает фактическую организацию реализации и не требует от каждой будущей возможности одинаковой глубины директорий.
