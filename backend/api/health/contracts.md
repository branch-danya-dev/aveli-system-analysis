# Health Contracts

## `GET /health`

Purpose:

Liveness check.

It does not verify external dependencies.

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

The backend verifies PostgreSQL availability using:

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
