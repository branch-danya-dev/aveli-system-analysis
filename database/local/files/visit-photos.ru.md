# Visit Photo Files

> Physical binary-file persistence, связанный с metadata `visit_photos`.

## Path Model

```text
documents/visit_photos/<sanitizedUserId>/<appointmentId>/...
```

SQLite row хранит metadata и `local_path`; байты изображения находятся вне SQLite.

## Ownership

Photo files следуют той же per-user workspace isolation rule, что и SQLite database.

## Удаление

Delete appointment каскадом удаляет metadata rows через FK.

Physical files чистятся отдельной repository/file-root логикой, а не SQLite.

Поэтому одной DB referential integrity недостаточно, чтобы гарантировать отсутствие orphaned files.

## Related

- [`../entities/visit_photos.ru.md`](../entities/visit_photos.ru.md)
