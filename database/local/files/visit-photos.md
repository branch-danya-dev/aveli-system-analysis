# Visit Photo Files

> Physical binary-file persistence associated with `visit_photos` metadata.

## Path Model

```text
documents/visit_photos/<sanitizedUserId>/<appointmentId>/...
```

The SQLite row stores metadata and `local_path`; the image bytes live outside SQLite.

## Ownership

Photo files follow the same per-user workspace isolation rule as the SQLite database.

## Deletion

Deleting an appointment cascades database metadata rows through the FK relationship.

Physical files are cleaned by separate repository/file-root logic rather than by SQLite itself.

This separation means DB referential integrity alone cannot guarantee absence of orphaned files.

## Related

- [`../entities/visit_photos.md`](../entities/visit_photos.md)
