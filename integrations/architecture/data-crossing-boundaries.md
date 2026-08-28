# Data Crossing External Boundaries

| Direction | Data | Authority / Source | Aveli Persistence | Sensitivity |
|---|---|---|---|---|
| Aveli Mobile → RevenueCat SDK | Aveli server user UUID | Aveli identity | RevenueCat customer identity | Medium |
| Apple/Google Store → RevenueCat | Purchase/subscription lifecycle evidence | Mobile store | Provider state | Medium |
| RevenueCat → Aveli Backend | Entitlement/subscription JSON | RevenueCat/provider evidence | `subscriptions` | Medium |
| RevenueCat webhook → Aveli Backend | Event metadata + sanitized payload | RevenueCat | `subscription_events` | Medium |
| Device Contacts → Frontend | Name, phone, contact id | User device | Drift `clients` | PII |
| Frontend → OS Notifications | Title/body/appointment id | Aveli scheduling intent | OS scheduler | Low |
| Camera/Gallery → Frontend | Selected image | User/device | Copied file + DB metadata | PII |
| Exchange API → Frontend | Currency rates | Third-party provider | `app_settings` cache | Low |
| Frontend → SMS App | Phone + message body | User decides send | None | PII |
| Frontend → Share Target | Export JSON file | User chooses target | Temporary file | PII |
| OS → Frontend | Connectivity types | OS hint only | Stream only | Low |

External data is authoritative only for the fact owned by that source. Store/RevenueCat state is billing evidence; Aveli backend decides workspace access. Connectivity is a hint, not proof of backend reachability. Imported contacts become Aveli-owned local client records.
