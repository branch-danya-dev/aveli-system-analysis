# Переменные окружения бэкенда

| Переменная | Значение по умолчанию | Назначение |
|---|---|---|
| `DATABASE_URL` | нет | Подключение к PostgreSQL. |
| `JWT_ACCESS_SECRET` | нет | Секрет подписи JWT доступа. |
| `JWT_ACCESS_TTL` | `15m` | Срок жизни токена доступа. |
| `JWT_REFRESH_TTL_DAYS` | `60` | Срок жизни сохраняемой сессии обновления, в днях. |
| `TRIAL_DAYS` | `30` | Продолжительность регистрационного пробного периода. |
| `SUBSCRIPTION_OFFLINE_GRACE_HOURS` | `72` | Ориентир периода повторной проверки, возвращаемый клиенту. |
| `REVENUECAT_SECRET_API_KEY` | нет | Серверные учётные данные RevenueCat REST. |
| `REVENUECAT_WEBHOOK_AUTH` | нет | Точное значение заголовка `Authorization` для вебхука. |
| `REVENUECAT_API_BASE` | `https://api.revenuecat.com` | Базовый адрес API RevenueCat. |
| `CORS_ORIGINS` | пусто | Разрешённые источники браузера, если CORS включён. |
| `PORT` | `3000` | Порт прослушивания. |
| `HOST` | `0.0.0.0` | Адрес прослушивания. |
| `AVELI_ENV` | нет | Обозначение среды, например тестовой (`staging`) или производственной (`production`). |

## Базовый адрес API во Flutter

клиент на Flutter использует:

```text
AVELI_API_BASE
```

Текущие примеры окружений:

```text
производственная среда: https://api.aveli.app
тестовая среда: https://api-staging.aveli.app
```

Это конфигурация клиента, а не переменная окружения NestJS из таблицы выше.

## Политика и константа

Значения по умолчанию:

```text
TRIAL_DAYS=30
SUBSCRIPTION_OFFLINE_GRACE_HOURS=72
```

управляются конфигурацией.

Документация не должна выдавать их за неизменяемые константы реализации.

При этом 30-дневный регистрационный пробный период дополнительно закреплён текущими бизнес-правилами.

## Классификация секретов

Секреты:

```text
DATABASE_URL
JWT_ACCESS_SECRET
REVENUECAT_SECRET_API_KEY
REVENUECAT_WEBHOOK_AUTH
```

Несекретная конфигурация:

```text
JWT_ACCESS_TTL
JWT_REFRESH_TTL_DAYS
TRIAL_DAYS
SUBSCRIPTION_OFFLINE_GRACE_HOURS
REVENUECAT_API_BASE
CORS_ORIGINS
PORT
HOST
AVELI_ENV
```

Конкретная платформа хранения секретов и процедура их ротации зависят от инфраструктуры развёртывания и не установлены имеющимися подтверждениями.
