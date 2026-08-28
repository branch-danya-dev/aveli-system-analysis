# REST API

> JSON/HTTP contract style used by the Aveli backend.

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

Account/access/billing routes use:

```text
/v1/...
```

Health routes are intentionally unversioned:

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

Protected routes use:

```http
Authorization: Bearer <accessToken>
```

RevenueCat webhook authentication also uses `Authorization`, but with an exact shared-secret header value rather than JWT authentication.

## Error Shape

Canonical HTTP error shape:

```json
{
  "code": "AUTH_INVALID_CREDENTIALS",
  "message": "Invalid email or password"
}
```

See:

[`../../api/error-model.md`](../../api/error-model.md)

## Boundary

The API is not a workspace-sync interface.

Canonical machine-readable contract:

[`../../api/openapi.yaml`](../../api/openapi.yaml)
