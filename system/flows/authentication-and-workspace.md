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

The backend account id is reused as the client-side identity anchor:

```text
users.id
  ├── secure user id
  ├── aveli_<userId>.sqlite
  ├── visit_photos/<userId>/
  ├── access snapshot key
  └── RevenueCat App User ID
```

This is a **linking identifier**, not shared data ownership.

## Registration Trial

Account registration creates one backend-controlled registration trial.

Reinstalling the application or deleting the local workspace does not create a new backend account trial.

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

Authentication does not cause clients/appointments/payments/photos to be synchronized to the backend.

That is outside the current system boundary.
