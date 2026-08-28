# Database

> Canonical documentation for Aveli data ownership, models, persistence boundaries, physical schemas, and persistence technologies.

## Purpose

`database/` explains **what data exists, who owns it, how it is modeled, where its canonical state lives, and how it is physically persisted**.

## Structure

```text
database/
├── architecture/
├── models/
│   ├── conceptual/
│   └── logical/
├── local/
├── server/
├── stack/
└── diagrams/
```

| Area | Responsibility |
|---|---|
| `architecture/` | Ownership, source-of-truth boundaries, isolation, and lifecycle. |
| `models/` | Conceptual and logical data models. |
| `local/` | Verified device-side persistence. |
| `server/` | Verified backend persistence. |
| `stack/` | Canonical persistence-technology documentation. |
| `diagrams/` | Cross-area database knowledge maps. |

## Reading Path

```text
Architecture
    ↓
Conceptual Model
    ↓
Logical Model
    ↓
Local / Server Physical Persistence
    ↓
Persistence Stack
```

Start with:

[`architecture/data-ownership.md`](architecture/data-ownership.md)

Visual map:

[`diagrams/database-map.puml`](diagrams/database-map.puml)

## Core Principle

> **Data documentation progresses from ownership to conceptual structure, from conceptual structure to logical structure, and only then to physical persistence.**

Physical models remain owned by the persistence component that stores the data.

## Documentation Rules

[`../rules.md`](../rules.md)
