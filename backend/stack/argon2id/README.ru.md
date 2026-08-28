# Argon2id

> Password-hashing technology Aveli account authentication.

## Role

Argon2id защищает stored passwords: вместо plaintext credentials сохраняется one-way hash.

## Почему подходит

Passwords — long-lived authentication secrets, поэтому им нужен deliberately expensive password-hashing function, а не быстрый general-purpose digest. Argon2id разработан для password hashing и использует memory-hard behavior.

## Contextual Usage

Registration/login behavior: [`../../auth/authentication.ru.md`](../../auth/authentication.ru.md)

Physical storage: [`../../../database/server/entities/users.ru.md`](../../../database/server/entities/users.ru.md)

Dummy-hash behavior для missing users относится к authentication/security usage, а не к canonical technology definition.

## Dependencies

Current runtime package: `argon2` 0.45.

## Limitations

Hash cost влияет на CPU/memory, параметры могут требовать tuning, а их изменение требует upgrade/rehash policy. Exact Argon2 parameters отсутствуют в supplied implementation description и здесь не придумываются.

## Replaceability

**Medium.** Замена требует compatibility с existing hashes, изменений verification, возможно opportunistic rehash и regression testing, но не public API change.

## Alternatives

Релевантные alternatives: bcrypt, scrypt и PBKDF2 где требуется. Historical ADR с formal comparison отсутствует.
