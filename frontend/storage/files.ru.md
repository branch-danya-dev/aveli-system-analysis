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

- image selection через `image_picker`;
- maximum edge 1920;
- quality 85;
- metadata row сохраняется в Drift;
- binary file хранится на filesystem.

## Cleanup

- delete appointment: DB cascade + repository file cleanup;
- delete profile: `VisitPhotosRoot.deleteForUser(userId)`;
- logout: files сохраняются.

Canonical persistence boundary:

[`../../database/local/files/`](../../database/local/files/)
