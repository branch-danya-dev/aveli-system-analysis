# System

> Cross-layer synthesis of the Aveli system.

## Purpose

`system/` provides the **nearest common system-level view** of Aveli.

It does not replace the canonical knowledge owned by:

```text
business/
database/
backend/
frontend/
integrations/
```

Instead, it explains how those perspectives work together as one system.

The system layer owns models that describe several components at once:

```text
system context
component relationships
runtime pipelines
trust boundaries
data movement
cross-component invariants
boundary-changing evolution
system-wide open questions
```

## Status

**Synthesis baseline in progress**

This branch is constructed from the current implementation-verified lower-level documentation.

It should become the main input for the final whole-repository polish and consistency review.

## System Shape

At the highest level Aveli contains two different responsibility domains:

```text
Professional Workspace
        +
Account / Access
```

They are connected by workspace access control, but they do not share data ownership.

```text
Independent Specialist
        ↓
   Aveli Mobile Client
      /            \
     /              \
Local Workspace     Account / Access API
     ↓                    ↓
SQLite + Files        Aveli Backend
                           ↓
                    PostgreSQL
                           ↕
                       RevenueCat
                           ↕
                Apple App Store / Google Play
```

External device services support the mobile workspace:

```text
Contacts
Notifications
Camera / Gallery
Share / File Picker
Exchange Rate API
```

## Structure

```text
system/
├── README.md
├── README.ru.md
│
├── architecture/
│   ├── context.*
│   ├── component-model.*
│   ├── boundaries.*
│   └── system-map.puml
│
├── flows/
│   ├── bootstrap-and-access.*
│   ├── authentication-and-workspace.*
│   ├── purchase-and-entitlement.*
│   ├── offline-workspace.*
│   ├── logout-and-profile-delete.*
│   └── system-runtime.puml
│
├── data/
│   └── ownership-and-movement.*
│
├── trust/
│   └── trust-model.*
│
├── invariants/
│   └── system-invariants.*
│
├── evolution/
│   └── boundary-changing-features.*
│
└── review/
    ├── synthesis-review.*
    └── open-questions.*
```

## Reading Path

```text
architecture/context
        ↓
architecture/component-model
        ↓
architecture/boundaries
        ↓
flows/
        ↓
data/
        ↓
trust/
        ↓
invariants/
        ↓
evolution/
        ↓
review/
```

## Canonicality Rule

A system document may summarize a lower-level fact when necessary to explain a cross-layer model, but it should link back to its canonical owner.

Examples:

```text
System says:
"Professional workspace data remain device-owned."

Canonical detail:
→ ../database/architecture/data-ownership.md


System says:
"Backend resolves access using lifetime → manual → subscription → trial."

Canonical detail:
→ ../backend/access/access-resolution.md


System says:
"Purchase does not directly unlock the workspace."

Canonical detail:
→ ../frontend/billing/
→ ../integrations/revenuecat/
→ ../backend/billing/
```

> **Storage is hierarchical. Knowledge is graph-based.**

## Documentation Rules

[`../rules.md`](../rules.md)
