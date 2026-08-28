# open.er-api.com Integration

## Client

```text
OpenErExchangeRateRepository
```

## Request

Base URL:

```text
https://open.er-api.com/v6/latest
```

Request:

```http
GET /v6/latest/{FROM}
```

`FROM` — uppercase ISO currency code.

## Response Usage

Client читает:

```text
result
rates[TO]
```

## Timeout / Retry

```text
timeout: 12 seconds
automatic retry: none
```

## Cache

Persistence:

```text
app_settings.exchangeRateCacheV1
```

JSON:

```text
{ from, to, rate, fetchedAt }
```

TTL:

```text
24 hours
```

## Failure Semantics

Typed failures:

```text
manualHint
serviceUnavailable
badResponse
pairMissing
pairManual
```

UI может предложить manual path, если provider conversion нельзя trusted.

Provider failure не меняет unrelated professional workspace data.

## Credentials

Current integration не использует API key.

## Consumer

Service используется при local profile currency change для conversion stored amounts.

Frontend implementation:

[`../../frontend/workspace/device-integrations.ru.md`](../../frontend/workspace/device-integrations.ru.md)
