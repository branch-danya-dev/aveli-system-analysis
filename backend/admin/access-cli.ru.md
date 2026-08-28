# Administrative Access CLI

## Invocation

```bash
npm run admin -- <command> [args] [--yes]
```

## Commands

| Command | Behavior |
|---|---|
| `inspect-access <email>` | Inspect user и effective access/grants. |
| `grant-lifetime <email>` | Создать lifetime/admin access grant. |
| `grant-days <email> <days>` | Создать temporary manual/admin access grant. |
| `revoke-access <email>` | Revoke lifetime + manual grants, сохранив trial history. |
| `soft-delete-user <email>` | Выполнить account soft deletion по email. |

Implementation:

```text
backend/src/admin/cli.ts
backend/src/admin/admin-access.service.ts
```

`AdminAccessService` CLI-only и не имеет current HTTP controller.

## Development Seed

```bash
npm run prisma:seed
```

Current seed behavior:

- upsert admin account;
- create lifetime access grant.

Safety:

```text
forbidden when NODE_ENV=production
requires SEED_ADMIN_PASSWORD
```

Seed — development tooling, а не production admin workflow.
