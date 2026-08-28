# `access_grants`

> Persisted non-subscription access grants.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Grant identifier. |
| `user_id` | UUID FK | Owning user; ON DELETE CASCADE. |
| `type` | AccessGrantType | `trial`, `lifetime`, `manual_temporary`. |
| `source` | AccessGrantSource | `registration`, `admin`, `support`. |
| `starts_at` | TIMESTAMPTZ | Beginning of the validity window. |
| `ends_at` | TIMESTAMPTZ nullable | End of validity; NULL only for lifetime. |
| `revoked_at` | TIMESTAMPTZ nullable | Early revocation time. |
| `created_at` | TIMESTAMPTZ | Creation timestamp. |
| `created_by` | TEXT nullable | Admin identifier / creation reason. |
| `note` | TEXT nullable | Additional comment. |

## Constraints

```text
ends_at IS NULL OR ends_at > starts_at
lifetime               → ends_at IS NULL
trial/manual_temporary → ends_at IS NOT NULL
```

Partial UNIQUE index:

```sql
CREATE UNIQUE INDEX access_grants_one_registration_trial_per_user
  ON access_grants(user_id)
  WHERE type = 'trial' AND source = 'registration';
```

This prevents a second registration-trial row for the same user even after revocation.

## Indexes

```text
user_id
(type, source)
```

The registration-trial partial UNIQUE index is documented separately above.

## Registration Trial Creation

Current implementation creates the registration trial during `register`:

```text
startsAt = now
endsAt   = now + TRIAL_DAYS
note     = 'registration trial'
```

`TRIAL_DAYS` is environment-controlled and currently defaults to 30 days.

## Related Documentation

- [`../constraints/invariants.md`](../constraints/invariants.md)
- [`../schema/enums.md`](../schema/enums.md)
