# `services`

> Physical service records в local workspace.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `id` | TEXT | NO | PK; UUID v4. |
| `name` | TEXT | NO | Название услуги. |
| `duration` | INTEGER | NO | Длительность в минутах. |
| `price` | INTEGER | NO | Цена в целых единицах валюты профиля. |
| `return_interval` | INTEGER | YES | Интервал возврата в минутах. |

## Verification Note

Source ER overview использует `duration_minutes` / `return_interval_minutes`, а detailed table definition — `duration` / `return_interval`. Здесь используются detailed names до прямой сверки с кодом.
