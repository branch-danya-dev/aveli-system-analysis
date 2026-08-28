# System

> Cross-layer synthesis of Aveli.

## Status

**Baseline: Stable**

The system baseline synthesizes stable business, database, backend, frontend and integration perspectives.

Remaining non-blocking items are explicitly classified in the closure register rather than left as ambiguous architecture assumptions.

## Purpose

`system/` owns models that describe several components at once:

```text
system context
component relationships
runtime pipelines
trust boundaries
data movement
cross-component invariants
boundary-changing evolution
failure/release synthesis
closure register
```

## System Shape

```text
Independent Specialist
        ↓
Aveli Mobile
   ├── Local professional workspace
   │      ↓
   │  SQLite + Files
   │
   └── Account / Access API
          ↓
      Aveli Backend
          ↓
      PostgreSQL
          ↕
      RevenueCat
          ↕
 Apple App Store / Google Play
```

## Structure

```text
system/
├── architecture/
├── flows/
├── data/
├── trust/
├── invariants/
├── evolution/
└── review/
```

> **Storage is hierarchical. Knowledge is graph-based.**

## Documentation Rules

[`../rules.md`](../rules.md)
