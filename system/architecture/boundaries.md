# Aveli — System Boundaries

## 1. Product Boundary

Aveli is a personal workspace for one independent specialist.

A feature that introduces shared professional-data ownership changes the system boundary.

## 2. Data Ownership Boundary

```text
Professional Workspace
→ device-local owner

Identity / Access
→ backend owner
```

Canonical model:

[`../../database/architecture/data-ownership.md`](../../database/architecture/data-ownership.md)

## 3. Trust Boundary

The mobile client is not authoritative for:

```text
session validity
trial creation
access-grant precedence
subscription normalization
online access decision
```

The backend is not authoritative for:

```text
clients
appointments
payments
visit notes
visit photos
local professional history
```

## 4. Internal Interface Boundary

```text
Flutter
↕
Aveli Backend
```

is an internal system interface.

It is not an external integration.

Canonical HTTP contracts:

[`../../backend/api/`](../../backend/api/)

## 5. External Integration Boundary

```text
Aveli
↕
RevenueCat / Stores / OS Services / Third-Party APIs
```

is owned by:

[`../../integrations/`](../../integrations/)

## 6. Access Boundary

Access decides whether the workspace may currently be opened.

Access does **not** own the workspace data.

```text
hasAccess = false
→ workspace unavailable
→ professional data unchanged
```

## 7. Local Identity Boundary

Authenticated backend `users.id` selects:

```text
aveli_<userId>.sqlite
visit_photos/<userId>/...
access snapshot <userId>
RevenueCat App User ID
```

The user id therefore links several components, but those components keep different data ownership.

## 8. Failure Boundary

External/backend failure must not silently corrupt or delete unrelated local workspace information.

This is a system-level resilience principle.
