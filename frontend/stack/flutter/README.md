# Flutter

> Mobile UI/runtime framework of the Aveli client.

## Role

Flutter owns application rendering, widget lifecycle, platform integration surface, and composition of the mobile experience.

## Verified Context

Aveli `0.2.2+4` uses Dart SDK `^3.7.0`; Flutter SDK itself is not pinned in `pubspec.yaml`.

The app is feature-first with a thin `app/` shell and shared `core/`.

## Replaceability

**Low.** Replacing Flutter means a full client rewrite: UI, routing, platform services, state integration, local persistence bindings and tests.

## Boundary

Product behavior remains canonical outside the framework. Flutter-specific implementation belongs in `frontend/`.
