# Backend

> Canonical documentation Aveli account/access backend.

## Статус

**Baseline: Stable**

Backend docs reconciled с verified NestJS implementation evidence, API contracts, persistence, access logic, billing integration и final review.

## Responsibility

```text
account identity
authentication
refresh sessions
trial / manual / lifetime grants
normalized subscription state
effective online workspace access
billing reconciliation
RevenueCat webhook processing
```

Professional workspace backend не stores/synchronizes.

## Areas

| Area | Responsibility |
|---|---|
| `architecture/` | Backend boundary. |
| `stack/` | Runtime technologies. |
| `api/` | HTTP/OpenAPI. |
| `auth/` | Session lifecycle. |
| `access/` | Effective access. |
| `billing/` | Reconciliation. |
| `errors/` | Error taxonomy. |
| `security/` | Security controls. |
| `configuration/` | Runtime config. |
| `admin/` | Admin CLI. |

501 auth routes — future contract stubs, не current shipped capability.

Canonical API: [`api/openapi.yaml`](api/openapi.yaml)

## Documentation Rules

[`../rules.ru.md`](../rules.ru.md)
