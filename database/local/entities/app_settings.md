# `app_settings`

> Key-value local settings persistence.

| Column | Type | NULL | Meaning |
|---|---|---|---|
| `key` | TEXT | NO | PK; setting key. |
| `value` | TEXT | NO | Serialized setting value. |

## Known Keys

| Key | Format | Purpose |
|---|---|---|
| `themeId` | enum name | Theme. |
| `profileName` | string | Specialist name. |
| `profilePhone` | string | Phone. |
| `profileEmail` | string | Local UI profile email. |
| `profileSpecialization` | string | Specialization. |
| `profileStudioName` | string | Studio name. |
| `profilePublicSlug` | string | Public slug. |
| `profileCurrencyCode` | ISO 4217 | Working currency; default RUB. |
| `currencyDefaultRubV1` | `'1'` | EUR→RUB migration flag. |
| `profileLocaleCode` | string | Locale; default `ru`. |
| `profileAccountLifecycle` | storage name | UI account lifecycle. |
| `profilePhoneVerified` | `'1'` / `'0'` | Local UI state. |
| `profileEmailVerified` | `'1'` / `'0'` | Local UI state. |
| `profilePublicListing` | storage name | Public listing state. |
| `visitRemindersEnabled` | `'1'` / `'0'` | Local reminders; default on. |
| `scheduleWorkingStartMinutes` | int string | Workday start. |
| `scheduleWorkingEndMinutes` | int string | Workday end. |
| `exchangeRateCacheV1` | JSON | Exchange-rate cache. |

The profile values here are local UI/settings data and are not a mirror of server `users`.
