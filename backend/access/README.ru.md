# Backend Access Resolution

> Server-side resolution того, может ли authenticated account открыть Aveli workspace.

## Назначение

`access/` владеет deterministic access decision.

Authentication устанавливает identity.

Access resolution оценивает entitlement state.

## Canonical Priority

```text
Lifetime
   ↓
Manual Grant
   ↓
Active Subscription
   ↓
Active Trial
   ↓
None
```

Первый valid source становится effective access source.

## Ответственность

Область владеет:

- чтением valid access sources;
- evaluation source validity;
- deterministic source priority;
- resolution granted/denied workspace access;
- trial/access expiration behavior на уровне backend decision.

Она не владеет local workspace data и не удаляет их.

## Навигация

- [`access-resolution.ru.md`](access-resolution.ru.md)
- [`access-resolution.puml`](access-resolution.puml)

Physical access persistence:

[`../../database/server/`](../../database/server/)
