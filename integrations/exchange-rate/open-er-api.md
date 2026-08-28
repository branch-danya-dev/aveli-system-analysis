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

`FROM` is uppercase ISO currency code.

## Response Usage

The client reads:

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

Typed failures include:

```text
manualHint
serviceUnavailable
badResponse
pairMissing
pairManual
```

The UI may instruct the user to continue manually when provider conversion cannot be trusted.

Provider failure does not mutate unrelated professional workspace data.

## Credentials

No API key is used by the current integration.

## Consumer

The service is used during local profile currency changes when stored amounts need conversion.

Frontend implementation:

[`../../frontend/workspace/device-integrations.md`](../../frontend/workspace/device-integrations.md)
