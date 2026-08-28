# Интеграция с open.er-api.com

## Клиент

```text
OpenErExchangeRateRepository
```

## Запрос

Базовый URL:

```text
https://open.er-api.com/v6/latest
```

Запрос:

```http
GET /v6/latest/{FROM}
```

`FROM` — код валюты ISO в верхнем регистре.

## Использование ответа

Клиент читает:

```text
result
rates[TO]
```

## Тайм-аут и повторные попытки

```text
тайм-аут: 12 секунд
автоматический повтор: отсутствует
```

## Кэш

Хранение:

```text
app_settings.exchangeRateCacheV1
```

JSON:

```text
{ from, to, rate, fetchedAt }
```

Срок жизни:

```text
24 часа
```

## Поведение при сбое

Типизированные ошибки:

```text
manualHint
serviceUnavailable
badResponse
pairMissing
pairManual
```

Интерфейс может предложить ручной сценарий, если результат конвертации провайдера нельзя считать надёжным.

Сбой провайдера не изменяет несвязанные данные профессионального рабочего пространства.

## Учётные данные

Текущая интеграция не использует ключ API.

## Потребитель

Сервис используется при смене валюты локального профиля для пересчёта сохранённых сумм.

Реализация во фронтенде:

[`../../frontend/workspace/device-integrations.ru.md`](../../frontend/workspace/device-integrations.ru.md)
