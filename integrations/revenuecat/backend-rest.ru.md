# RevenueCat — граница REST API бэкенда

## Шлюз

Файлы бэкенда:

```text
backend/src/billing/revenuecat/revenuecat.gateway.ts
backend/src/billing/revenuecat/revenuecat.mapper.ts
backend/src/billing/subscription-sync.service.ts
```

## Конечная точка

Базовый адрес:

```text
REVENUECAT_API_BASE
по умолчанию: https://api.revenuecat.com
```

Запрос:

```http
GET /v1/subscribers/{app_user_id}
Authorization: Bearer <REVENUECAT_SECRET_API_KEY>
```

`app_user_id` — UUID аутентифицированного пользователя Aveli.

Клиент не передаёт отдельный идентификатор RevenueCat при синхронизации биллинга.

## Классификация результата

| Результат RevenueCat | Значение для бэкенда |
|---|---|
| HTTP 200 | `ok` → преобразовать данные подписчика в нормализованный снимок. |
| HTTP 404 | `not_found`. |
| отсутствует секрет / ошибка сети / другой ответ со статусом, отличным от OK | `unavailable`. |

В подтверждённой реализации нет явной повторной попытки шлюза и явно заданного тайм-аута запроса.

## Право доступа

Преобразователь читает:

```text
subscriber.entitlements[support]
```

Каноническое право доступа:

```text
support
```

## Сверка состояния

### `ok`

Преобразовать состояние провайдера и обновить либо создать нормализованную запись подписки.

### `not_found`

Операция обновления или вставки:

```text
status = expired
```

### `unavailable`

При отказе доступ не расширяется:

```text
502 BILLING_SYNC_FAILED
```

Новая операция обновления или вставки не выполняется.

Ранее сохранённая строка подписки может оставаться в PostgreSQL до успешной сверки; неудачная синхронизация не помечает её как заново подтверждённую.

## Граница полномочий

REST API RevenueCat предоставляет подтверждение состояния подписки со стороны провайдера.

После сверки бэкенд Aveli выполняет общий алгоритм определения доступа.

Локальная реализация бэкенда:

[`../../backend/billing/subscription-sync.ru.md`](../../backend/billing/subscription-sync.ru.md)
