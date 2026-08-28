# `subscription_events`

> Сохранённые записи обработки событий подписки и вебхуков.

| Поле | Тип | Назначение |
|---|---|---|
| `id` | UUID PK | Идентификатор записи события. |
| `subscription_id` | UUID FK, допускает NULL | Связанная строка `subscriptions`; ON DELETE SET NULL. |
| `user_id` | UUID FK, допускает NULL | Связанная запись `users`; ON DELETE SET NULL. |
| `provider` | TEXT | Провайдер события; обычно `revenuecat`. |
| `external_event_id` | TEXT UNIQUE | Внешний ключ идемпотентности события вебхука. |
| `event_type` | TEXT | Тип события провайдера. |
| `payload` | JSONB | Исходная полезная нагрузка провайдера. |
| `received_at` | TIMESTAMPTZ | Время получения события. |
| `processed_at` | TIMESTAMPTZ, допускает NULL | Время завершения обработки. |
| `processing_error` | TEXT, допускает NULL | Ошибка обработки, если возникла. |

## Инвариант

`external_event_id` имеет ограничение UNIQUE и служит основой идемпотентности вебхуков на уровне хранения.

Внешние ключи, допускающие NULL, с `SET NULL` позволяют сохранить историю событий после удаления связанного пользователя или подписки.

## Связанная документация

- [`../constraints/invariants.ru.md`](../constraints/invariants.ru.md)
