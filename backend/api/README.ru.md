# Backend API

> Canonical HTTP contract Aveli backend.

## Contract Ownership

`api/` владеет:

- paths и methods;
- authentication requirements;
- request bodies;
- response bodies;
- HTTP statuses;
- stable public JSON shapes;
- public error shape.

Internal service logic принадлежит `auth/`, `access/`, `billing/` и другим owning backend areas.

## Base

```text
release:  https://api.aveli.app
staging:  https://api-staging.aveli.app
prefix:   /v1
format:   JSON
```

Health endpoints unversioned.

## Contract Areas

- [`auth/`](auth/)
- [`access/`](access/)
- [`billing/`](billing/)
- [`health/`](health/)
- [`error-model.ru.md`](error-model.ru.md)

Machine-readable contract:

[`openapi.yaml`](openapi.yaml)

## Stable Client-Dependent Shapes

Current client compatibility зависит от:

- `AuthTokensResponse`;
- `AccessStatusView`;
- `{ code, message }` error JSON;
- `POST /v1/billing/sync` returning `AccessStatusView`;
- entitlement id `support`.

Изменение этих contracts является client-breaking без coordinated Flutter update.

## Validation

Global body validation:

```text
whitelist = true
forbidNonWhitelisted = true
transform = true
```

Unknown request-body fields → HTTP 400.

## Deprecated / Absent Contract

Current `/v1/subscription` controller отсутствует.

Canonical subscription/access routes:

```text
GET  /v1/access
POST /v1/billing/sync
```
