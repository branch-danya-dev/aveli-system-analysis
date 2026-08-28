# Aveli — Data Lifecycle

> Поведение persistent data при важных состояниях продукта Aveli.

## Основной принцип

Aveli разделяет:

```text
Identity state
Access state
Workspace state
```

Изменение одного состояния не должно неявно уничтожать данные другого.

> **Logout или access expiration могут сделать workspace неактивным или недоступным, но не должны удалять persistent professional data.**

## Workspace Lifecycle

```text
Workspace created / first used
        ↓
Professional data created and updated
        ↓
Workspace remains persistent
        ↓
May become inactive after logout
        ↓
May become unavailable after access loss
        ↓
Can become active again for the same user
```

## Logout

`BR-025`, `BR-043`–`BR-046` требуют завершения authenticated state при сохранении persistent workspace information.

## Account Switching

`BR-047` требует isolation:

```text
Workspace A inactive
        ↓
User B becomes active
        ↓
Workspace B active
```

Workspace data между пользователями не переносятся.

## Access Expiration

`BR-026` требует:

```text
Access expires
    ↓
Workspace blocked
    ↓
Persistent data preserved
    ↓
Access restored
    ↓
Same workspace available again
```

## Client Lifecycle

Известное правило (`BR-040`):

```text
Active Client
    ↓ archive
Archived Client
    ↓
Historical information remains preserved
```

Правила permanent deletion еще не финализированы и не должны придумывацца здесь.

## Appointment / Visit Lifecycle

Известные business states: `Scheduled`, `Cancelled`, `No-show`, `Completed`. Completed work может содержать notes, photos и payment state.

Database layer сохраняет необходимые различия, но не переопределяет transition rules.

## Payment Lifecycle

Completed visit может быть paid или outstanding (`BR-034`–`BR-038`). Точные persisted states и transitions должны быть проверены по реализации.

## Workspace Media

Visit photos и другие user-specific workspace media должны оставаться изолированными и сохраняться при logout и access expiration. Точное file deletion, orphan cleanup и retention behavior остаются открытыми до проверки реализации.

## Открытые вопросы

- permanent client deletion;
- service deactivation или deletion;
- точные payment transitions;
- import/export conflict behavior;
- visit-photo deletion и orphan cleanup;
- backup и restore behavior.

Видимый gap лучше выдуманного правила.

## Связанная документация

- [`data-ownership.ru.md`](data-ownership.ru.md)
- [`../../business/requirements/business-rules.ru.md`](../../business/requirements/business-rules.ru.md)
- [`../../business/processes/`](../../business/processes/)
