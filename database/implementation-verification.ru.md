# Database Implementation Verification

> Record сверки logical data model с предоставленным physical persistence description.

## Статус

**Applied**

Перечисленные ниже implementation facts уже применены к текущей logical и physical database documentation.

Этот файл является audit/verification record, а не еще одной canonical копией data model.

## Примененные результаты проверки

Подтверждено и учтено:

- local workspace использует отдельную SQLite database на authenticated user;
- local business entity identifiers — UUID v4;
- local entities не содержат foreign keys на server user UUID;
- appointment имеет максимум одну physical payment row;
- partial payment хранится через `amount_paid` в этой строке;
- appointment хранит start и end timestamps;
- appointment хранит snapshot цены Service;
- appointment хранит denormalized `payment_status`;
- persistence Service содержит return interval;
- visit-photo metadata содержит ссылки на appointment и client;
- app settings используют key-value TEXT persistence;
- server access grants и subscription state физически разделены;
- uniqueness registration trial enforced в PostgreSQL;
- subscription state хранится как RevenueCat snapshot, а raw events — отдельно.

Canonical result находится в:

- [`models/logical/data-model.ru.md`](models/logical/data-model.ru.md)
- [`local/`](local/)
- [`server/`](server/)

## Findings, требующие Product Classification

Physical persistence содержит concepts, которые пока слабо отражены в business documentation:

- `confirmed` appointment status;
- business significance Service `returnInterval`;
- profile public slug / public listing;
- local phone/email verification flags;
- local account lifecycle setting;
- exchange-rate cache persistence;
- backend `disabled` / `deleted` user states;
- provider subscription states `trialing`, `past_due`, `grace_period`, `revoked`.

До продвижения в business documentation каждый concept нужно классифицировать:

```text
current product requirement
implementation support state
legacy/future capability
```

Эти findings не блокируют database baseline: physical state описан без придумывания business meaning.

## Known Source Naming Discrepancy

В предоставленном local persistence description встречаются два варианта имен Service duration fields:

```text
ER overview:      duration_minutes / return_interval_minutes
Detailed table:  duration / return_interval
```

Текущая physical documentation использует detailed table definition:

```text
duration
return_interval
```

Расхождение остается явно зафиксированным до прямой проверки Drift table declarations.

Оно влияет на уверенность в именах, но не на logical meaning полей.

## Baseline Result

Database branch считается **Stable / baseline-ready с одним явно документированным source naming discrepancy**.

Будущие implementation changes должны сначала обновлять owning physical document, а затем инициировать logical/traceability review там, где это требуется.
