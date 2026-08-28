# API Error Model

> Canonical wire representation of backend HTTP errors.

## Shape

```json
{
  "code": "AUTH_INVALID_CREDENTIALS",
  "message": "Invalid email or password"
}
```

`ApiExceptionFilter` normalizes errors into this shape.

## Common HTTP Mapping

| HTTP | Typical Meaning |
|---:|---|
| 400 | DTO / generic validation failure |
| 401 | Invalid credentials, invalid JWT, expired/invalid refresh session |
| 403 | Disabled/deleted account state where exposed as `AUTH_USER_DISABLED` |
| 409 | Registration email conflict |
| 500 | Internal error |
| 501 | Declared auth stub is not implemented |
| 502 | Billing synchronization cannot verify RevenueCat state |
| 503 | Readiness dependency failure |

## Canonical Error Codes

Backend code ownership:

[`../errors/error-codes.md`](../errors/error-codes.md)

## Access Denial Is Not an HTTP Error

A valid authenticated request to:

```text
GET /v1/access
```

returns HTTP 200 even when workspace access is denied.

Denial is represented by:

```json
{
  "hasAccess": false,
  "source": "none",
  "reason": "..."
}
```

This distinction must remain stable:

```text
technical/auth failure
        ≠
valid product access denial
```
