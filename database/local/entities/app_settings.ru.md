# `app_settings`

> Key-value persistence локальных настроек.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `key` | TEXT | NO | PK; ключ настройки. |
| `value` | TEXT | NO | Serialized value настройки. |

## Known Keys

| Key | Format | Назначение |
|---|---|---|
| `themeId` | enum name | Theme. |
| `profileName` | string | Имя специалиста. |
| `profilePhone` | string | Телефон. |
| `profileEmail` | string | Локальный UI profile email. |
| `profileSpecialization` | string | Специализация. |
| `profileStudioName` | string | Название студии. |
| `profilePublicSlug` | string | Public slug. |
| `profileCurrencyCode` | ISO 4217 | Рабочая валюта; default RUB. |
| `currencyDefaultRubV1` | `'1'` | Migration flag EUR→RUB. |
| `profileLocaleCode` | string | Locale; default `ru`. |
| `profileAccountLifecycle` | storage name | UI account lifecycle. |
| `profilePhoneVerified` | `'1'` / `'0'` | Local UI state. |
| `profileEmailVerified` | `'1'` / `'0'` | Local UI state. |
| `profilePublicListing` | storage name | Public listing state. |
| `visitRemindersEnabled` | `'1'` / `'0'` | Local reminders; default on. |
| `scheduleWorkingStartMinutes` | int string | Начало рабочего дня. |
| `scheduleWorkingEndMinutes` | int string | Конец рабочего дня. |
| `exchangeRateCacheV1` | JSON | Cache курса валют. |

Profile values здесь — local UI/settings data, а не зеркало server `users`.
