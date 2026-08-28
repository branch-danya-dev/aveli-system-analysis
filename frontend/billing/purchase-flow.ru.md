# Сценарий покупки и восстановления на мобильном клиенте

## Сервис RevenueCat

```text
RevenueCatPurchaseService
```

`NoopPurchaseService` используется, если мобильные ключи RevenueCat не заданы.

## Конфигурация

Параметры `dart-define`:

```text
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Право доступа по умолчанию:

```text
support
```

Идентификаторы продуктов не зашиты во Flutter; они загружаются из предложений RevenueCat и магазина приложений.

## Идентификация пользователя

После аутентификации и активации рабочего пространства:

```text
Purchases.logIn(userId)
```

где `userId` — серверный UUID.

## Покупка

```text
Экран подписки
  ↓
Purchases.purchase
  ↓
результат покупки
  ↓
AccessController.syncBilling
  ↓
POST /v1/billing/sync
  ↓
серверный AccessStatusView
  ↓
AccessState + защищённый снимок
```

## Восстановление покупки

```text
Purchases.restorePurchases
  ↓
syncBilling
  ↓
серверное состояние доступа
```

## Граница полномочий

Клиент может читать `CustomerInfo.entitlements[support].isActive` как часть результата покупки, но это не открывает рабочее пространство напрямую.

Рабочее пространство открывается только после сверки состояния на бэкенде.

## Модель отслеживания изменений

Глобальный `addCustomerInfoUpdateListener` не используется.

При возвращении приложения на передний план клиент один раз получает актуальный `CustomerInfo` и обновляет либо синхронизирует состояние доступа.

## Состояния результата покупки

```text
успех
сверка ожидается (`syncPending`)
ожидание (`pending`)
отменено (`cancelled`)
```
