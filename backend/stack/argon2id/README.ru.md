# Argon2id

> Password hashing mechanism Aveli account authentication.

## Verified Usage

Registration сохраняет:

```text
password
   ↓ Argon2id
password_hash
```

Current package:

```text
argon2 0.45
```

Plaintext passwords не сохраняются.

## Login Verification

Backend проверяет supplied credential по stored Argon2id hash.

Failed login для missing user использует dummy hash, чтобы уменьшить account-existence timing leakage.

Public invalid-credential outcome намеренно unified для:

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

Exact Argon2 parameters в текущем implementation description не указаны и здесь не выдумываются.

## Contextual Usage

[`../../auth/authentication.ru.md`](../../auth/authentication.ru.md)
