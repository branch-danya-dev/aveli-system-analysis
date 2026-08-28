# Local SQLite Enumerations

> Application enums, сохраняемые в local SQLite как TEXT значения enum `.name`.

| Enum | Persisted values |
|---|---|
| `AppointmentStatus` | `scheduled`, `confirmed`, `completed`, `cancelled`, `noShow` |
| `PaymentStatus` | `unpaid`, `partial`, `paid` |
| `PaymentMethod` | `cash`, `card`, `transfer` |
| `VisitPhotoType` | `before`, `after`, `general` |

## Persistence Rule

Значения хранятся как TEXT через application enum name.

Поэтому rename enum value может стать persistence compatibility change и должен рассматриваться вместе с migration behavior.

## Product Classification Note

Наличие persisted enum value не делает его автоматически canonical business state.

Например, `confirmed` существует в physical appointment persistence, но требует product-level classification до продвижения в business process documentation.
