# Authentication and Local Workspace Binding

## Registration / Login

```text
User credentials
    ↓
Flutter Auth
    ↓
Aveli Backend
    ↓
users + auth_sessions
    ↓
access / refresh credentials
    ↓
Secure Storage
    ↓
activate workspace using users.id
```

## Identity Link

Backend account id используется как client-side identity anchor:

```text
users.id
  ├── secure user id
  ├── aveli_<userId>.sqlite
  ├── visit_photos/<userId>/
  ├── access snapshot key
  └── RevenueCat App User ID
```

Это **linking identifier**, а не shared data ownership.

## Registration Trial

Account registration создает один backend-controlled registration trial.

Reinstall app или delete local workspace не создает новый backend trial.

## Separation of Responsibilities

Backend:

```text
Who is the user?
Is the session valid?
What access sources exist?
```

Frontend:

```text
Which local workspace belongs to this authenticated user?
How should the user enter that workspace?
```

## No Workspace Upload

Authentication не синхронизирует clients/appointments/payments/photos в backend.

Это вне current system boundary.
