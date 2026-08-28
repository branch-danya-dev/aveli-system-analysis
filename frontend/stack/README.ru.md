# Стек фронтенда

> Каноническая документация технологий, используемых мобильным клиентом Aveli во время выполнения.

## Подтверждённый стек

| Технология | Версия | Роль |
|---|---:|---|
| Flutter / Dart | Dart SDK `^3.7.0` | Мобильный интерфейс и среда выполнения. |
| `flutter_riverpod` | 2.6.1 | Управление состоянием и зависимостями. |
| `go_router` | 14.8.1 | Маршрутизация, перенаправления и оболочка приложения. |
| Drift | 2.34.3 | Типизированный слой доступа к SQLite. |
| `http` | 1.6.0 | Транспорт HTTP для обращения к бэкенду. |
| `flutter_secure_storage` | 11.0.0 | Хранение учётных данных и снимка состояния доступа. |
| `purchases_flutter` | 10.9.1 | Мобильный SDK RevenueCat. |
| `flutter_local_notifications` | 22.3.0 | Локальные напоминания о визитах. |
| `connectivity_plus` | 7.3.1 | Сигнал о типе подключения. |
| `flutter_contacts` | 2.3.1 | Импорт контактов только для чтения. |
| `image_picker` | 1.1.2 | Выбор фотографий визитов. |

## Канонические документы технологий

- [`flutter/`](flutter/)
- [`riverpod/`](riverpod/)
- [`go-router/`](go-router/)
- [`drift/`](drift/)
- [`http/`](http/)
- [`secure-storage/`](secure-storage/)
- [`revenuecat-sdk/`](revenuecat-sdk/)
- [`local-notifications/`](local-notifications/)

SQLite — технология хранения данных → [`../../database/stack/sqlite/`](../../database/stack/sqlite/)

Drift — технология доступа к данным на стороне Flutter → [`drift/`](drift/)
