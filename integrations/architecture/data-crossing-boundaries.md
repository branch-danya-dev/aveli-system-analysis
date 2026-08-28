# Data Crossing External Boundaries

| Direction | Data | External Authority | Aveli Persistence | Sensitivity |
|---|---|---|---|---|
| Aveli → RevenueCat SDK | Aveli server user UUID | Aveli identity | RevenueCat customer record | Medium |
| RevenueCat/store → backend | Entitlement/subscription JSON | RevenueCat/store | `subscriptions` | Medium |
| RevenueCat webhook → backend | Event metadata + sanitized payload | RevenueCat | `subscription_events` | Medium |
| Device contacts → frontend | Name, phone, device contact id | User device | Drift `clients` | PII |
| Frontend → OS notifications | Title, body, appointment id | Aveli scheduling intent | OS scheduler | Low |
| Camera/gallery → frontend | Selected image | User/device | Copied file + DB metadata | PII |
| Exchange API → frontend | Currency rates | Third party | `app_settings` cache | Low |
| Frontend → SMS app | Phone + message body | User decides send | None in integration | PII |
| Frontend → share target | Export JSON file | User chooses target | Temp file only | PII |
| OS → frontend | Connectivity types | OS | Stream only | Low |

## Authority Notes

Data crossing a boundary does not automatically become authoritative for every Aveli decision.

Examples:

- RevenueCat subscription data is authoritative provider evidence, but Aveli backend decides workspace access.
- Connectivity type is only a network hint, not proof of backend reachability.
- Device contacts are imported source data; Aveli owns the resulting local client record.
