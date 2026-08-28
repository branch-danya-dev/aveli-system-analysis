# Backend API

> Canonical HTTP contract owned by the Aveli backend.

## Contract Ownership

`api/` owns:

- paths and methods;
- authentication requirements;
- request bodies;
- response bodies;
- HTTP statuses;
- stable public JSON shapes;
- public error shape.

Internal service logic belongs to `auth/`, `access/`, `billing/`, and other owning backend areas.

## Base

```text
release:  https://api.aveli.app
staging:  https://api-staging.aveli.app
prefix:   /v1
format:   JSON
```

Health endpoints are unversioned.

## Contract Areas

- [`auth/`](auth/)
- [`access/`](access/)
- [`billing/`](billing/)
- [`health/`](health/)
- [`error-model.md`](error-model.md)

Machine-readable contract:

[`openapi.yaml`](openapi.yaml)

## Stable Client-Dependent Shapes

Current client compatibility depends on:

- `AuthTokensResponse`;
- `AccessStatusView`;
- `{ code, message }` error JSON;
- `POST /v1/billing/sync` returning `AccessStatusView`;
- entitlement id `support`.

Changing these is a client-breaking contract change unless the Flutter consumer is updated in the same coordinated change.

## Validation

Global body validation:

```text
whitelist = true
forbidNonWhitelisted = true
transform = true
```

Unknown request-body fields produce HTTP 400.

## Deprecated / Absent Contract

There is no current `/v1/subscription` controller.

Canonical subscription/access routes are:

```text
GET  /v1/access
POST /v1/billing/sync
```
