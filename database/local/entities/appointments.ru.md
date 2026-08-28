# `appointments`

> Центральная physical scheduling record.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK. |
| `client_id` | TEXT | NO | FK → `clients.id`; ON DELETE CASCADE не указан. |
| `service_id` | TEXT | NO | FK → `services.id`. |
| `starts_at` | DATETIME | NO | Начало слота. |
| `ends_at` | DATETIME | NO | Конец слота. |
| `price` | INTEGER | NO | Snapshot цены на момент записи. |
| `status` | TEXT | NO | `AppointmentStatus`. |
| `payment_status` | TEXT | NO | Агрегированный `PaymentStatus` для UI. |
| `created_at` | DATETIME | NO | Создание. |
| `updated_at` | DATETIME | NO | Обновление. |

## Индексы

- `appointments_starts_at(starts_at)` — Calendar / Today.
- `appointments_client_id(client_id)` — история client.

## Денормализация

`price` намеренно копируется из Service на момент записи, поэтому последующее изменение service price не переписывает историю.
