# RevenueCat — граница мобильного SDK

## Технология

```text
purchases_flutter 10.9.1
```

Основной клиентский класс:

```text
RevenueCatPurchaseService
```

## Инициализация

RevenueCat настраивается лениво.

`ensureConfigured()` вызывается при первом:

```text
logIn
getOfferings
purchase
restore
refreshCustomerInfo
```

При холодном запуске начальная инициализация не выполняет предварительную настройку RevenueCat.

Если публичные ключи обеих платформ не заданы, фронтенд использует:

```text
NoopPurchaseService
```

## Публичная конфигурация

```text
REVENUECAT_IOS_API_KEY
REVENUECAT_ANDROID_API_KEY
REVENUECAT_ENTITLEMENT_ID
```

Право доступа по умолчанию:

```text
support
```

Ключи мобильного SDK — публичная конфигурация приложения, а не секреты бэкенда.

## Предложения и продукты

Идентификаторы производственных продуктов не зашиты во Flutter.

Клиент загружает:

```text
Purchases.getOfferings()
→ current.availablePackages
```

Отображение месячного и годового вариантов формируется из метаданных пакета и продукта RevenueCat.

Актуальная цена магазина отображается из данных продукта, полученных от провайдера:

```text
storeProduct.priceString
storeProduct.price
```

## Сценарий покупки

```text
Экран подписки
  ↓
Purchases.purchase(...)
  ↓
CustomerInfo / результат покупки
  ↓
AccessController.syncBilling
  ↓
POST /v1/billing/sync
  ↓
серверный AccessStatusView
```

Клиентское состояние права доступа RevenueCat само по себе не открывает рабочее пространство.

## Сценарий восстановления

```text
Purchases.restorePurchases()
  ↓
syncBilling
  ↓
серверное состояние доступа
```

## Поведение при возврате приложения

Глобальный `addCustomerInfoUpdateListener` не используется.

При возвращении приложения на передний план фронтенд явно обновляет данные пользователя RevenueCat, а затем обновляет или синхронизирует доступ.

## Подтверждённые состояния результата

```text
успех
отменено (`cancelled`)
ожидание (`pending`)
ошибка
недоступно
```

Реализация компонента:

[`../../frontend/billing/purchase-flow.ru.md`](../../frontend/billing/purchase-flow.ru.md)
