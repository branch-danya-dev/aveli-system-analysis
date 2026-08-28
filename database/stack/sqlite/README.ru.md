# SQLite

> Local relational storage engine профессионального workspace Aveli.

## Role

SQLite — physical database engine каждого local professional workspace пользователя.

```text
User
  ↓
aveli_<userId>.sqlite
  ↓
clients / services / appointments / payments / notes / photo metadata / settings
```

Database остается на устройстве и не синхронизируется с Aveli backend.

## Почему подходит Aveli

Workspace Aveli имеет relational, а не document-oriented model.

Текущая schema использует:

- foreign keys между appointments, clients и services;
- `UNIQUE(appointment_id)` для одной payment row на appointment;
- cascade behavior для visit-related records;
- joins для Today, Calendar, client history и outstanding payments;
- versioned schema с 11 migration versions;
- переносимый per-user `.sqlite` file.

Поэтому важный architectural choice — не просто «SQLite вместо другого Flutter package».

Это:

> **relational persistence вместо document/key-value persistence для professional workspace.**

## Почему не Hive / Isar для текущей модели

Hive хорошо подходит для lightweight key-value state и cache, но workspace Aveli зависит от relational constraints и joins.

Isar может моделировать связанные данные, однако его внедрение потребует заменить существующие constraints, migrations, reactive data access и user database format.

Это migration storage model, а не простая замена library.

## Dependencies

Canonical physical usage:

- [`../../local/schema/`](../../local/schema/)
- [`../../local/entities/`](../../local/entities/)
- [`../../local/migrations/`](../../local/migrations/)

Consumer technology:

- [`../drift/`](../drift/)

## Replaceability

**Replaceability: medium на уровне engine/file format, но низкий приоритет замены для текущего продукта.**

Замена SQLite затронет:

- migration пользовательских данных;
- relationships и constraints;
- Calendar / Today query projections;
- payment invariants;
- import/export;
- persistence tests.

Замена Drift при сохранении SQLite значительно дешевле, чем замена самого SQLite.

## Alternatives

Релевантные альтернативы:

- Isar;
- Hive;
- другой SQLite access layer, например `sqflite`.

В репозитории пока нет formal historical ADR, подтверждающего, что они были формально рассмотрены до implementation.
