# Argon2id

> Password-hashing technology used by Aveli account authentication.

## Role

Argon2id protects stored passwords by persisting a one-way hash instead of plaintext credentials.

## Why It Fits

Passwords are long-lived authentication secrets and require a deliberately expensive password-hashing function rather than a fast general-purpose digest. Argon2id is designed for password hashing and provides memory-hard behavior.

## Contextual Usage

Registration/login behavior: [`../../auth/authentication.md`](../../auth/authentication.md)

Physical storage: [`../../../database/server/entities/users.md`](../../../database/server/entities/users.md)

The dummy-hash behavior for missing users belongs to authentication/security usage, not this canonical technology definition.

## Dependencies

Current runtime package: `argon2` 0.45.

## Limitations

Hash cost affects CPU/memory usage, parameters may require future tuning, and a parameter change needs an upgrade/rehash policy. Exact Argon2 parameters are not present in the supplied implementation description and are intentionally not invented.

## Replaceability

**Medium.** A change requires compatibility with existing hashes, verification changes, possible opportunistic rehash and regression testing, but not a public API change.

## Alternatives

Relevant alternatives include bcrypt, scrypt and PBKDF2 where required. No historical ADR records a formal comparison.
