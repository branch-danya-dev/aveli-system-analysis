# Frontend Stack

> Canonical runtime technology knowledge for the Aveli Flutter client.

## Verified Runtime Stack

| Technology | Version | Role |
|---|---:|---|
| Flutter / Dart | Dart SDK `^3.7.0` | Mobile UI/runtime. |
| `flutter_riverpod` | 2.6.1 | State/dependency management. |
| `go_router` | 14.8.1 | Routing/redirects/shell. |
| Drift | 2.34.3 | Typed SQLite data-access layer. |
| `http` | 1.6.0 | Backend HTTP transport. |
| `flutter_secure_storage` | 11.0.0 | Credentials/access snapshot. |
| `purchases_flutter` | 10.9.1 | RevenueCat mobile SDK. |
| `flutter_local_notifications` | 22.3.0 | Local visit reminders. |
| `connectivity_plus` | 7.3.1 | Connectivity hint. |
| `flutter_contacts` | 2.3.1 | Read-only contact import. |
| `image_picker` | 1.1.2 | Visit-photo selection. |

## Canonical Technology Documents

- [`flutter/`](flutter/)
- [`riverpod/`](riverpod/)
- [`go-router/`](go-router/)
- [`drift/`](drift/)
- [`http/`](http/)
- [`secure-storage/`](secure-storage/)
- [`revenuecat-sdk/`](revenuecat-sdk/)
- [`local-notifications/`](local-notifications/)

SQLite is a storage engine → [`../../database/stack/sqlite/`](../../database/stack/sqlite/)

Drift is Flutter-side data access → [`drift/`](drift/)
