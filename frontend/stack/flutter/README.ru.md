# Flutter

> Mobile UI/runtime framework Aveli client.

## Role

Flutter владеет application rendering, widget lifecycle, platform integration surface и composition mobile experience.

## Verified Context

Aveli `0.2.2+4` использует Dart SDK `^3.7.0`; Flutter SDK не pinned в `pubspec.yaml`.

App — feature-first с thin `app/` shell и shared `core/`.

## Replaceability

**Low.** Замена Flutter означает full client rewrite: UI, routing, platform services, state integration, local persistence bindings и tests.

## Boundary

Product behavior остается canonical вне framework. Flutter-specific implementation принадлежит `frontend/`.
