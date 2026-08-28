# Frontend Stack

> Canonical technology knowledge current Aveli Flutter client.

## Verified Runtime Stack

| Technology | Version | Role |
|---|---:|---|
| Flutter / Dart | Dart SDK `^3.7.0` | Mobile UI/runtime. |
| `flutter_riverpod` | 2.6.1 | State management и dependency injection. |
| `go_router` | 14.8.1 | Routing, shell navigation и redirects. |
| Drift | 2.34.3 | Typed local SQLite data-access layer. |
| `http` | 1.6.0 | Backend account/access HTTP transport. |
| `flutter_secure_storage` | 11.0.0 | Session credentials и access snapshot. |
| `purchases_flutter` | 10.9.1 | RevenueCat mobile purchase/restore integration. |
| `flutter_local_notifications` | 22.3.0 | Local visit reminders. |
| `connectivity_plus` | 7.3.1 | Connectivity hint для access/offline behavior. |
| `flutter_contacts` | 2.3.1 | Read-only device contact import. |
| `image_picker` | 1.1.2 | Visit photo selection. |

Supporting packages: `timezone`, `flutter_timezone`, `file_picker`, `path_provider`, `path`, `uuid`, `intl`, `share_plus`, `url_launcher`.

## Canonical Technology Documents

- [`flutter/`](flutter/)
- [`riverpod/`](riverpod/)
- [`go-router/`](go-router/)
- [`drift/`](drift/)
- [`http/`](http/)
- [`secure-storage/`](secure-storage/)
- [`revenuecat-sdk/`](revenuecat-sdk/)
- [`local-notifications/`](local-notifications/)

Меньшие integration libraries документируются contextually в owning usage areas.

## Ownership Note

SQLite остается database technology.

Drift — Flutter-side typed data-access technology и canonical здесь, с contextual links на [`../../database/local/`](../../database/local/).

Older duplicate Drift technology docs в `database/` лучше убрать во время final whole-system polish, не создавая новые дубли.
