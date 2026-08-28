# `appointments`

> Центральная физическая запись расписания.

| Столбец | Тип | NULL | Назначение |
|---|---|---|---|
| `id` | TEXT | NO | Первичный ключ. |
| `client_id` | TEXT | NO | Внешний ключ → `clients.id`; каскадное удаление в исходном описании не указано. |
| `service_id` | TEXT | NO | Внешний ключ → `services.id`. |
| `starts_at` | DATETIME | NO | Начало временного интервала. |
| `ends_at` | DATETIME | NO | Окончание временного интервала. |
| `price` | INTEGER | NO | Снимок цены на момент создания записи. |
| `status` | TEXT | NO | `AppointmentStatus`. |
| `payment_status` | TEXT | NO | Агрегированный `PaymentStatus` для интерфейса. |
| `created_at` | DATETIME | NO | Время создания. |
| `updated_at` | DATETIME | NO | Время обновления. |

## Индексы

- `appointments_starts_at(starts_at)` — Calendar / Today.
- `appointments_client_id(client_id)` — история клиента.

## Денормализация

`price` намеренно копируется из услуги в момент создания записи, поэтому последующее изменение цены услуги не переписывает историю.
