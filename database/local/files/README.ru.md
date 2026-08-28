# Local File Persistence

> Binary и file-based persistence, принадлежащий local professional workspace.

## Назначение

`files/` описывает persistent workspace data, которые находятся вне SQLite, но остаются частью той же local ownership boundary.

## Ответственность

Область документирует:

- file-location strategy;
- связь database metadata с binary files;
- per-user isolation;
- deletion и cleanup behavior;
- file-lifecycle constraints.

## Навигация

- [`visit-photos.ru.md`](visit-photos.ru.md)

Связанная SQLite metadata:

[`../entities/visit_photos.ru.md`](../entities/visit_photos.ru.md)

Frontend-specific filesystem implementation позже должен ссылаться на эту canonical persistence boundary, а не переопределять ее.
