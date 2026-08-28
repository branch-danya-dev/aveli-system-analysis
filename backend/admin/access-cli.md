# Administrative Access CLI

## Invocation

```bash
npm run admin -- <command> [args] [--yes]
```

## Commands

| Command | Behavior |
|---|---|
| `inspect-access <email>` | Inspect user and effective access/grants. |
| `grant-lifetime <email>` | Create lifetime/admin access grant. |
| `grant-days <email> <days>` | Create temporary manual/admin access grant. |
| `revoke-access <email>` | Revoke lifetime + manual grants while preserving trial history. |
| `soft-delete-user <email>` | Perform account soft deletion by email. |

Implementation:

```text
backend/src/admin/cli.ts
backend/src/admin/admin-access.service.ts
```

`AdminAccessService` is CLI-only and has no current HTTP controller.

## Development Seed

```bash
npm run prisma:seed
```

Current seed behavior:

- upsert an admin account;
- create lifetime access grant.

Safety:

```text
forbidden when NODE_ENV=production
requires SEED_ADMIN_PASSWORD
```

The seed is development tooling, not a production admin workflow.
