# `access_grants`

> Persisted non-subscription access grants.

| Field | Type | Meaning |
|---|---|---|
| `id` | UUID PK | Идентификатор grant. |
| `user_id` | UUID FK | Owning user; ON DELETE CASCADE. |
| `type` | AccessGrantType | `trial`, `lifetime`, `manual_temporary`. |
| `source` | AccessGrantSource | Источник grant: `registration`, `admin`, `support`. |
| `starts_at` | TIMESTAMPTZ | Начало validity window. |
| `ends_at` | TIMESTAMPTZ nullable | Конец validity; NULL только для lifetime. |
| `revoked_at` | TIMESTAMPTZ nullable | Время досрочного revoke. |
| `created_at` | TIMESTAMPTZ | Время создания. |
| `created_by` | TEXT nullable | Admin identifier / причина создания. |
| `note` | TEXT nullable | Дополнительный комментарий. |

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

Он не позволяет создать вторую registration-trial row для того же user даже после revoke.

## Индексы

```text
user_id
(type, source)
```

Partial UNIQUE registration-trial index описан отдельно выше.

## Создание Registration Trial

Текущая implementation создает registration trial при `register`:

```text
startsAt = now
endsAt   = now + TRIAL_DAYS
note     = 'registration trial'
```

`TRIAL_DAYS` задается environment configuration и сейчас имеет default 30 дней.

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
- [`../schema/enums.ru.md`](../schema/enums.ru.md)
