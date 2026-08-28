# `access_grants`

> Сохранённые права доступа, не связанные напрямую с подпиской.

| Поле | Тип | Назначение |
|---|---|---|
| `id` | UUID PK | Идентификатор права доступа. |
| `user_id` | UUID FK | Пользователь-владелец; ON DELETE CASCADE. |
| `type` | `AccessGrantType` | `trial`, `lifetime`, `manual_temporary`. |
| `source` | `AccessGrantSource` | Источник: `registration`, `admin`, `support`. |
| `starts_at` | TIMESTAMPTZ | Начало срока действия. |
| `ends_at` | TIMESTAMPTZ, допускает NULL | Конец срока действия; NULL только для пожизненного доступа. |
| `revoked_at` | TIMESTAMPTZ, допускает NULL | Время досрочного отзыва. |
| `created_at` | TIMESTAMPTZ | Время создания. |
| `created_by` | TEXT, допускает NULL | Идентификатор администратора или причина создания. |
| `note` | TEXT, допускает NULL | Дополнительный комментарий. |

## Ограничения

```text
ends_at IS NULL OR ends_at > starts_at
lifetime               → ends_at IS NULL
trial/manual_temporary → ends_at IS NOT NULL
```

Частичный уникальный индекс:

```sql
CREATE UNIQUE INDEX access_grants_one_registration_trial_per_user
  ON access_grants(user_id)
  WHERE type = 'trial' AND source = 'registration';
```

Он не позволяет создать второй регистрационный пробный период для того же пользователя даже после отзыва предыдущего.

## Индексы

```text
user_id
(type, source)
```

Частичный уникальный индекс регистрационного пробного периода описан отдельно выше.

## Создание регистрационного пробного периода

Текущая реализация создаёт регистрационный пробный период при `register`:

```text
startsAt = now
endsAt   = now + TRIAL_DAYS
note     = 'registration trial'
```

`TRIAL_DAYS` задаётся через конфигурацию окружения и сейчас по умолчанию равен 30 дням.

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
- [`../schema/enums.ru.md`](../schema/enums.ru.md)
