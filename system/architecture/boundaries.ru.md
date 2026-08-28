# Aveli — System Boundaries

## 1. Product Boundary

Aveli — personal workspace одного independent specialist.

Feature, вводящий shared professional-data ownership, меняет system boundary.

## 2. Data Ownership Boundary

```text
Professional Workspace
→ device-local owner

Identity / Access
→ backend owner
```

Canonical:

[`../../database/architecture/data-ownership.ru.md`](../../database/architecture/data-ownership.ru.md)

## 3. Trust Boundary

Mobile client не authoritative для:

```text
session validity
trial creation
access-grant precedence
subscription normalization
online access decision
```

Backend не authoritative для:

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

— internal system interface, не external integration.

Canonical HTTP contracts:

[`../../backend/api/`](../../backend/api/)

## 5. External Integration Boundary

```text
Aveli
↕
RevenueCat / Stores / OS Services / Third-Party APIs
```

Canonical:

[`../../integrations/`](../../integrations/)

## 6. Access Boundary

Access решает, можно ли открыть workspace сейчас.

Access **не** владеет workspace data.

```text
hasAccess = false
→ workspace unavailable
→ professional data unchanged
```

## 7. Local Identity Boundary

Authenticated backend `users.id` выбирает:

```text
aveli_<userId>.sqlite
visit_photos/<userId>/...
access snapshot <userId>
RevenueCat App User ID
```

User id связывает несколько components, но data ownership остается разным.

## 8. Failure Boundary

External/backend failure не должен silently corrupt/delete unrelated local workspace information.

Это system-level resilience principle.
