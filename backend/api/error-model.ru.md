# API Error Model

> Canonical wire representation backend HTTP errors.

## Shape

```json
{
  "code": "AUTH_INVALID_CREDENTIALS",
  "message": "Invalid email or password"
}
```

`ApiExceptionFilter` нормализует errors в этот shape.

## Common HTTP Mapping

| HTTP | Typical Meaning |
|---:|---|
| 400 | DTO / generic validation failure |
| 401 | Invalid credentials, invalid JWT, expired/invalid refresh session |
| 403 | Disabled/deleted account state через `AUTH_USER_DISABLED` |
| 409 | Registration email conflict |
| 500 | Internal error |
| 501 | Declared auth stub не реализован |
| 502 | Billing synchronization не может проверить RevenueCat state |
| 503 | Readiness dependency failure |

## Canonical Error Codes

Backend code ownership:

[`../errors/error-codes.ru.md`](../errors/error-codes.ru.md)

## Access Denial Is Not an HTTP Error

Valid authenticated request:

```text
GET /v1/access
```

возвращает HTTP 200 даже при denied workspace access.

Denial представлен:

```json
{
  "hasAccess": false,
  "source": "none",
  "reason": "..."
}
```

Важно:

```text
technical/auth failure
        ≠
valid product access denial
```
