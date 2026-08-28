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

Professional workspace data между пользователями не переносятся.

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
Same preserved workspace available again
```

## Reinstall / Local Data Loss

Текущая persistence model имеет важную границу:

```text
Reinstall
    ↓
Local application data могут быть потеряны
    ↓
Тот же account выполняет sign in
    ↓
Может быть создан новый пустой aveli_<userId>.sqlite
```

Server-controlled registration trial state независим от local database и **не сбрасывается reinstall**.

Legacy `aveli.db` не привязывается к account автоматически; без explicit claim/migration path данные остаются неприсвоенными.

Это следствие текущей local-first ownership model. При появлении cloud backup или workspace synchronization правило необходимо пересмотреть.

## Client Lifecycle

Известное правило (`BR-040`):

```text
Active Client
    ↓ archive
Archived Client
    ↓
Historical information remains preserved
```

Permanent client deletion остается product-level open question.

## Appointment / Visit Lifecycle

Известные business states: `Scheduled`, `Cancelled`, `No-show`, `Completed`.

Physical implementation также содержит `confirmed`. Его product significance нужно классифицировать до продвижения в canonical business behavior.

Completed work может содержать notes, photos и payment state.

## Payment Lifecycle

Physical persistence подтверждена:

```text
Appointment 1 → 0..1 Payment

PaymentStatus:
unpaid
partial
paid
```

Partial payment хранится в той же строке через `amount_paid`.

Открытым остается **business transition policy**: какие transitions разрешены, когда они происходят и существуют ли правила correction/reversal.

## Workspace Media

Visit-photo persistence разделен между database metadata и device files:

```text
visit_photos row
    +
documents/visit_photos/<userId>/<appointmentId>/...
```

Delete appointment каскадом удаляет связанные `visit_photos` metadata rows.

Physical files очищаются отдельно repository/file-root логикой (`VisitPhotosRoot.deleteForUser` входит в документированное implementation behavior).

Открытыми остаются policy-level guarantees: orphan cleanup coverage, retention, backup и recovery.

## Открытые вопросы lifecycle

- permanent client deletion;
- service deactivation или deletion;
- business rules payment correction/transitions;
- import/export conflict behavior;
- orphan-file guarantees и media retention;
- backup и restore behavior.

Видимый gap лучше выдуманного правила.

## Связанная документация

- [`data-ownership.ru.md`](data-ownership.ru.md)
- [`../local/`](../local/)
- [`../server/`](../server/)
- [`../../business/requirements/business-rules.ru.md`](../../business/requirements/business-rules.ru.md)
- [`../../business/processes/`](../../business/processes/)
