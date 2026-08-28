# Database

> Canonical documentation for Aveli data ownership, models, persistence boundaries, physical schemas, and persistence technologies.

## Purpose

`database/` explains **what data exists, who owns it, how it is modeled, which persistence technologies are used, where its canonical state lives, and how it is physically persisted**.

## Status

**Baseline: Stable**

The current database documentation has been reconciled with the supplied persistence implementation description.

One known source-level naming discrepancy remains for the local Service duration fields and is explicitly documented in the local schema. This does not change the documented data meaning.

## Structure

```text
database/
├── architecture/
├── models/
│   ├── conceptual/
│   └── logical/
├── stack/
├── local/
├── server/
└── diagrams/
```

| Area | Responsibility |
|---|---|
| `architecture/` | Ownership, source-of-truth boundaries, isolation, and lifecycle. |
| `models/` | Conceptual and logical data models. |
| `stack/` | Canonical persistence-technology decisions and replaceability. |
| `local/` | Verified device-side persistence usage. |
| `server/` | Verified backend persistence usage. |
| `diagrams/` | Cross-area database knowledge maps. |

## Reading Path

```text
Architecture
    ↓
Conceptual Model
    ↓
Logical Model
    ↓
Persistence Stack
    ↓
Local / Server Physical Usage
```

This follows the repository rule that canonical stack knowledge is documented before contextual technology usage.

Start with:

[`architecture/data-ownership.md`](architecture/data-ownership.md)

Verification record:

[`implementation-verification.md`](implementation-verification.md)

Visual map:

[`diagrams/database-map.puml`](diagrams/database-map.puml)

## Core Principle

> **Data documentation progresses from ownership to conceptual structure, from conceptual structure to logical structure, and only then to technology selection and physical persistence usage.**

Physical models remain owned by the persistence component that stores the data.

## Documentation Rules

[`../rules.md`](../rules.md)
