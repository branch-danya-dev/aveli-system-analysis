# Frontend Stack

> Canonical technology knowledge for the current Aveli Flutter client.

## Verified Runtime Stack

| Technology | Version | Role |
|---|---:|---|
| Flutter / Dart | Dart SDK `^3.7.0` | Mobile UI/runtime. |
| `flutter_riverpod` | 2.6.1 | State management and dependency injection. |
| `go_router` | 14.8.1 | Routing, shell navigation and redirects. |
| Drift | 2.34.3 | Typed local SQLite data-access layer. |
| `http` | 1.6.0 | Backend account/access HTTP transport. |
| `flutter_secure_storage` | 11.0.0 | Session credentials and access snapshot. |
| `purchases_flutter` | 10.9.1 | RevenueCat mobile purchase/restore integration. |
| `flutter_local_notifications` | 22.3.0 | Local visit reminders. |
| `connectivity_plus` | 7.3.1 | Connectivity hint for access/offline behavior. |
| `flutter_contacts` | 2.3.1 | Read-only device contact import. |
| `image_picker` | 1.1.2 | Visit photo selection. |

Supporting packages also include `timezone`, `flutter_timezone`, `file_picker`, `path_provider`, `path`, `uuid`, `intl`, `share_plus`, and `url_launcher`.

## Canonical Technology Documents

- [`flutter/`](flutter/)
- [`riverpod/`](riverpod/)
- [`go-router/`](go-router/)
- [`drift/`](drift/)
- [`http/`](http/)
- [`secure-storage/`](secure-storage/)
- [`revenuecat-sdk/`](revenuecat-sdk/)
- [`local-notifications/`](local-notifications/)

Smaller integration libraries are documented in their usage context rather than receiving mandatory technology directories.

## Ownership Note

SQLite remains a database technology.

Drift is the Flutter-side typed data-access technology and is therefore canonical here, with contextual links to [`../../database/local/`](../../database/local/).

Any older duplicate Drift technology documentation under `database/` should be handled during final whole-system polish rather than duplicated further.
