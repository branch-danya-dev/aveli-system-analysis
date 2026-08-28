# `access_grants`

> Persisted non-subscription access grants.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Grant identifier. |
| `user_id` | UUID FK | → users; ON DELETE CASCADE. |
| `type` | AccessGrantType | trial / lifetime / manual_temporary. |
| `source` | AccessGrantSource | registration / admin / support. |
| `starts_at` | TIMESTAMPTZ | Validity start. |
| `ends_at` | TIMESTAMPTZ nullable | Validity end; null only for lifetime. |
| `revoked_at` | TIMESTAMPTZ nullable | Early revocation. |
| `created_at` | TIMESTAMPTZ | Creation. |
| `created_by` | TEXT nullable | Admin id / reason. |
| `note` | TEXT nullable | Comment. |

## Constraints

```text
ends_at IS NULL OR ends_at > starts_at
lifetime              → ends_at IS NULL
trial/manual_temporary → ends_at IS NOT NULL
```

Partial UNIQUE index:

```sql
CREATE UNIQUE INDEX access_grants_one_registration_trial_per_user
  ON access_grants(user_id)
  WHERE type = 'trial' AND source = 'registration';
```

This is the persistence-level enforcement preventing a second registration trial row for the same user, including after revocation.
