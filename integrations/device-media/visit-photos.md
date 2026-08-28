# Device Media → Aveli Visit Photo

## Technology

```text
image_picker 1.1.2
```

Entry:

```text
addVisitPhoto
visit_photo_picker.dart
```

## Sources

Supported:

```text
camera
gallery
```

## Native Permission Evidence

Android:

```text
CAMERA
```

iOS:

```text
NSCameraUsageDescription
NSPhotoLibraryUsageDescription
```

## Media Processing

Picker constraints:

```text
max edge: 1920 px
quality: 85
```

The selected `XFile` path is temporary external input.

Aveli copies it into its own workspace persistence:

```text
visit_photos/<userId>/<appointmentId>/<uuid>.<ext>
```

Drift persists metadata including local path.

## Ownership Transition

```text
Device media
→ external/temporary source

Copied Aveli visit photo
→ Aveli-owned local professional data
```

## Cleanup

- appointment deletion: DB cascade + file cleanup;
- profile deletion: user photo tree deleted;
- logout: photos preserved.

## Permission Failure

Permission/access errors may surface as:

```text
PhotoAccessDenied
```

with user-facing recovery messaging.

Frontend usage:

[`../../frontend/storage/files.md`](../../frontend/storage/files.md)
