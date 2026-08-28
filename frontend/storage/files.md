# Local File Usage

## Visit Photos

Root:

```text
documents/visit_photos/<sanitizedUserId>/
```

Per appointment:

```text
<appointmentId>/<photoId>.<ext>
```

## Client Usage

- image selection through `image_picker`;
- maximum edge 1920;
- quality 85;
- metadata row persisted in Drift;
- binary file stored on filesystem.

## Cleanup

- deleting an appointment: DB cascade plus repository file cleanup;
- deleting profile: `VisitPhotosRoot.deleteForUser(userId)`;
- logout: files are preserved.

Canonical persistence boundary:

[`../../database/local/files/`](../../database/local/files/)
