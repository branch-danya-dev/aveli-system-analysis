# Backend Authentication

> Server-side identity и authenticated-session lifecycle.

## Назначение

`auth/` описывает, как backend устанавливает **кто этот account**.

Authentication намеренно отделена от access resolution.

```text
Authentication
    ↓
Кто пользователь?

Access
    ↓
Может ли пользователь открыть workspace?
```

## Ответственность

Область владеет backend behavior для:

- account registration;
- credential validation;
- sign-in;
- authenticated session creation;
- access-token issuance;
- refresh-token rotation;
- session expiration/revocation;
- logout/revocation semantics.

## Навигация

- [`authentication.ru.md`](authentication.ru.md)
- [`session-lifecycle.ru.md`](session-lifecycle.ru.md)
- [`auth-flow.puml`](auth-flow.puml)

Access decision:

[`../access/`](../access/)
