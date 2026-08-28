# REST API

> JSON/HTTP contract style Aveli backend.

## Base Addresses

Current environment examples:

```text
release:  https://api.aveli.app
staging:  https://api-staging.aveli.app
```

Client base configuration key:

```text
AVELI_API_BASE
```

## Versioning

Account/access/billing routes используют:

```text
/v1/...
```

Health routes intentionally unversioned:

```text
/health
/ready
```

## Representation

```http
Content-Type: application/json
Accept: application/json
```

## Authentication

Protected routes:

```http
Authorization: Bearer <accessToken>
```

RevenueCat webhook также использует `Authorization`, но это exact shared-secret header value, а не JWT auth.

## Error Shape

Canonical HTTP error shape:

```json
{
  "code": "AUTH_INVALID_CREDENTIALS",
  "message": "Invalid email or password"
}
```

См.:

[`../../api/error-model.ru.md`](../../api/error-model.ru.md)

## Boundary

API не является workspace-sync interface.

Canonical machine-readable contract:

[`../../api/openapi.yaml`](../../api/openapi.yaml)
