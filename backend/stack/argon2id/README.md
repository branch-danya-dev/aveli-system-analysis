# Argon2id

> Password hashing mechanism used by Aveli account authentication.

## Verified Usage

Registration stores:

```text
password
   ↓ Argon2id
password_hash
```

Current package:

```text
argon2 0.45
```

Plaintext passwords are not persisted.

## Login Verification

The backend verifies the supplied credential against the stored Argon2id hash.

Failed login for a missing user uses a dummy hash to reduce account-existence timing leakage.

This means the public invalid-credential outcome is intentionally unified for:

```text
missing user
wrong password
disabled/unavailable account during credential handling
```

Public contract:

```text
401 AUTH_INVALID_CREDENTIALS
```

## Boundary

Exact Argon2 parameters are not provided by the current implementation description and are therefore not documented here.

## Contextual Usage

[`../../auth/authentication.md`](../../auth/authentication.md)
