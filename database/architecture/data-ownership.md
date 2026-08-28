# Aveli — Data Ownership

> Canonical ownership and source-of-truth boundaries for Aveli data.

## Core Boundary

```text
Professional Workspace
        ↓
   User Workspace

Identity & Access
        ↓
      Backend
```

The domains are connected by access control, but they do not share ownership.

> **Professional work data belongs to the isolated user workspace. Identity and access state belong to the backend-controlled account domain.**

This boundary is derived from `BR-020`–`BR-026` and `BR-043`–`BR-047`.

## Professional Workspace Domain

| Data | Canonical Owner |
|---|---|
| Clients | Professional workspace |
| Services | Professional workspace |
| Appointments | Professional workspace |
| Payments | Professional workspace |
| Visit notes | Professional workspace |
| Visit photos / workspace media | Professional workspace |

The current product model does not make the Aveli backend the source of truth for these records.

Workspace data is isolated per user. Data from one workspace must not become visible while another user's workspace is active.

## Identity & Access Domain

| Data | Canonical Owner |
|---|---|
| Account identity | Backend |
| Authenticated session authority | Backend |
| Trial state | Backend |
| Manual access grants | Backend |
| Lifetime access | Backend |
| Resolved subscription state | Backend |
| Effective workspace access decision | Backend |

Subscription evidence may originate from the mobile billing ecosystem, but Aveli's effective access decision remains a backend-controlled product responsibility.

## Access Does Not Own Workspace Data

```text
Valid Access → Workspace may be opened
No Access     → Workspace unavailable
                ↓
              Persistent workspace data remains unchanged
```

Access expiration must not delete or modify persistent professional workspace information.

## Logout and Account Switching

Logout ends the active authenticated state but does not delete the persistent workspace.

Account switching changes the active workspace context; it does not transfer data between workspaces.

## External Data Authorities

External origin does not automatically imply ownership of the resulting Aveli record. For example, importing a device contact creates or enriches an Aveli client record without making the device contact the owner of that Aveli record.

## Boundary-Changing Features

This model must be revisited for:
- multi-device workspace synchronization;
- cloud workspace backup;
- public booking backed by server-side professional data;
- shared or collaborative workspaces.

## Related Documentation

- [`../../business/requirements/business-rules.md`](../../business/requirements/business-rules.md)
- [`data-lifecycle.md`](data-lifecycle.md)
- [`../models/conceptual/domain-model.md`](../models/conceptual/domain-model.md)
