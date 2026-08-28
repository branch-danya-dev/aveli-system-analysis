# Local File Persistence

> Binary and file-based persistence owned by the local professional workspace.

## Purpose

`files/` documents persistent workspace data that lives outside SQLite while remaining part of the same local ownership boundary.

## Responsibility

This area owns documentation of:

- file-location strategy;
- relation between database metadata and binary files;
- per-user isolation;
- deletion and cleanup behavior;
- file-lifecycle constraints.

## Navigation

- [`visit-photos.md`](visit-photos.md)

Related SQLite metadata:

[`../entities/visit_photos.md`](../entities/visit_photos.md)

Frontend-specific filesystem implementation should later reference this canonical persistence boundary rather than redefine it.
