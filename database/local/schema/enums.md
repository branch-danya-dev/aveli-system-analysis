# Local SQLite Enumerations

> Application enums persisted as TEXT enum `.name` values in the local SQLite database.

| Enum | Persisted values |
|---|---|
| `AppointmentStatus` | `scheduled`, `confirmed`, `completed`, `cancelled`, `noShow` |
| `PaymentStatus` | `unpaid`, `partial`, `paid` |
| `PaymentMethod` | `cash`, `card`, `transfer` |
| `VisitPhotoType` | `before`, `after`, `general` |

## Persistence Rule

The values are stored as TEXT using the application enum name.

Changing an enum name can therefore become a persistence compatibility change and must be reviewed together with migration behavior.

## Product Classification Note

The existence of a persisted enum value does not automatically make it a canonical business state.

For example, `confirmed` exists in physical appointment persistence but still requires product-level classification before being promoted into business process documentation.
