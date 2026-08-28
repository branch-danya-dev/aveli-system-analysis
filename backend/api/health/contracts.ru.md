# Health Contracts

## `GET /health`

Purpose:

Liveness check.

External dependencies не проверяются.

Success:

```json
{
  "status": "ok"
}
```

HTTP:

```text
200
```

## `GET /ready`

Purpose:

Readiness check.

Backend проверяет PostgreSQL через:

```sql
SELECT 1
```

Success:

```json
{
  "status": "ok"
}
```

HTTP:

```text
200
```

Dependency failure:

```json
{
  "status": "error"
}
```

HTTP:

```text
503
```
