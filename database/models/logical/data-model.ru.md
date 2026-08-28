# Aveli — Logical Data Model

> Technology-independent logical representation информации, которую Aveli должна сохранять.

---

## Назначение

Logical model уточняет conceptual domain до attributes, identifiers, relationships и cardinalities, необходимых текущему product behavior.

Она учитывает проверенную physical persistence model, но не привязана к SQLite, PostgreSQL, Drift, Prisma или конкретным SQL types.

---

## Статус модели

Документ теперь является **verified baseline logical model**.

Physical persistence подтвердила:

- максимум одну physical payment row на appointment;
- partial payment внутри этой строки;
- хранение start и end timestamps appointment;
- price snapshot appointment;
- aggregated payment status appointment;
- service return interval;
- ссылки visit-photo metadata на appointment и client;
- physical key-value persistence workspace settings;
- отдельное persistence access grants и subscription state на backend.

---

# Identity & Access Domain

## Account

Logical attributes:

| Attribute | Meaning |
|---|---|
| `id` | Stable account identifier. |
| `email` | Sign-in identity при email authentication. |
| `status` | Account lifecycle state, где требуется. |

```text
Account 1 → 0..* Session
Account 1 → 0..* Access Source
Account 1 → 0..* Subscription State
```

Professional workspace entities остаются вне backend account ownership domain.

---

## Session

| Attribute | Meaning |
|---|---|
| `id` | Stable session identifier. |
| `accountId` | Owning Account. |
| `validUntil` | Session validity boundary. |
| `state` | Session lifecycle state. |
| `deviceContext` | Optional device/platform context. |

Token hashes и storage mechanics относятся к physical/backend layer.

---

## Access Source

| Attribute | Meaning |
|---|---|
| `id` | Stable source identifier. |
| `accountId` | Account-владелец. |
| `type` | Lifetime / Manual Grant / Trial / другой source. |
| `source` | Origin grant, где применимо. |
| `startsAt` | Начало validity. |
| `expiresAt` | Конец validity. |
| `revokedAt` | Early revocation. |

Effective `Access` остается derived concept, а не обязательной persisted entity.

---

## Subscription State

| Attribute | Meaning |
|---|---|
| `id` | Stable subscription-state identifier. |
| `accountId` | Related Account. |
| `product` | Store product reference. |
| `entitlement` | Logical entitlement. |
| `status` | Current subscription state. |
| `autoRenew` | Auto-renew state. |
| `validUntil` | Validity boundary. |
| `lastVerifiedAt` | Последняя provider verification. |

Raw provider payloads не входят в logical subscription model.

---

## Subscription Event

| Attribute | Meaning |
|---|---|
| `id` | Stable event identifier. |
| `subscriptionId` | Related Subscription State, где определено. |
| `accountId` | Related Account, где определено. |
| `externalEventId` | External idempotency identifier. |
| `eventType` | Provider event type. |
| `payload` | Raw provider event content. |
| `receivedAt` | Время получения. |
| `processedAt` | Время обработки. |
| `processingError` | Ошибка обработки. |

---

# Professional Workspace Domain

## Client

| Attribute | Meaning |
|---|---|
| `id` | Stable client identifier. |
| `name` | Display name. |
| `phone` | Phone/contact value. |
| `social` | Social/messenger contact. |
| `deviceContactId` | Imported device-contact reference. |
| `tags` | Classification metadata. |
| `archivedAt` | Archive timestamp. |
| `createdAt` | Creation timestamp. |

```text
Client 1 → 0..* Appointment
```

---

## Service

| Attribute | Meaning |
|---|---|
| `id` | Stable service identifier. |
| `name` | Service name. |
| `duration` | Expected duration. |
| `price` | Current default service price. |
| `returnInterval` | Return interval, где настроен. |

```text
Service 1 → 0..* Appointment
```

---

## Appointment

| Attribute | Meaning |
|---|---|
| `id` | Stable appointment identifier. |
| `clientId` | Related Client. |
| `serviceId` | Related Service. |
| `startsAt` | Planned start. |
| `endsAt` | Planned end. |
| `priceSnapshot` | Цена, сохраненная независимо от последующих изменений Service. |
| `status` | Appointment lifecycle state. |
| `paymentStatus` | Aggregated payment state. |
| `createdAt` | Creation timestamp. |
| `updatedAt` | Update timestamp. |

Relationships:

```text
Client      1 → 0..* Appointment
Service     1 → 0..* Appointment
Appointment 1 → 0..* Visit Note
Appointment 1 → 0..* Visit Photo
Appointment 1 → 0..1 Payment
```

`Appointment 1 → 0..1 Payment` теперь подтвержден physical UNIQUE constraint.

Conceptual `Visit` остается valid domain concept, хотя persistence хранит visit-related data через appointment-associated records.

---

## Visit Note

| Attribute | Meaning |
|---|---|
| `id` | Stable note identifier. |
| `appointmentId` | Appointment/visit context. |
| `body` | Note content. |
| `createdAt` | Creation timestamp. |

---

## Visit Photo

| Attribute | Meaning |
|---|---|
| `id` | Stable photo metadata identifier. |
| `appointmentId` | Appointment/visit context. |
| `clientId` | Related Client. |
| `mediaReference` | Logical reference на stored file. |
| `type` | Before / after / general. |
| `createdAt` | Creation timestamp. |

Physical file path остается в physical persistence layer.

---

## Payment

| Attribute | Meaning |
|---|---|
| `id` | Stable payment identifier. |
| `appointmentId` | Related Appointment. |
| `amount` | Amount due. |
| `amountPaid` | Уже оплаченная сумма. |
| `status` | Unpaid / partial / paid. |
| `method` | Payment method. |
| `createdAt` | Creation timestamp. |
| `paidAt` | Full/partial payment timestamp. |

Verified cardinality:

```text
Appointment 1 → 0..1 Payment
```

Partial payment хранится внутри одной Payment record, а не несколькими rows.

---

## Workspace Settings

Logical groups:

- specialist profile values;
- working currency;
- locale;
- appearance/theme;
- working schedule;
- reminders;
- supported local/public-profile state;
- exchange-rate cache.

Это logical grouping, а не требование одной normalized physical entity.

---

# Logical Relationships

```text
IDENTITY & ACCESS

Account
 ├── 0..* Session
 ├── 0..* Access Source
 ├── 0..* Subscription State
 └── 0..* Subscription Event

Effective Access
    ↑ derived from Access Source + Subscription State
    │
    └── controls workspace availability


PROFESSIONAL WORKSPACE

Client
  1
  └── 0..* Appointment

Service
  1
  └── 0..* Appointment

Appointment
  1
  ├── 0..* Visit Note
  ├── 0..* Visit Photo
  └── 0..1 Payment

Workspace Settings
  └── affects local workspace/application behavior
```

---

# Open / Classification Questions

Physical implementation выявила concepts, которые нужно классифицировать до продвижения в business layer:

- `confirmed` appointment status;
- business significance `returnInterval`;
- profile public slug / public listing;
- local phone/email verification flags;
- local account lifecycle setting;
- exchange-rate cache persistence;
- backend `disabled` / `deleted` account states;
- subscription states `past_due`, `grace_period`, `revoked`, `trialing`.

Классификация:

```text
current product requirement
implementation support state
legacy/future capability
```

---

## Связанная документация

- [`../conceptual/domain-model.ru.md`](../conceptual/domain-model.ru.md)
- [`../../architecture/data-ownership.ru.md`](../../architecture/data-ownership.ru.md)
- [`../../architecture/data-lifecycle.ru.md`](../../architecture/data-lifecycle.ru.md)
- [`../../local/`](../../local/)
- [`../../server/`](../../server/)
