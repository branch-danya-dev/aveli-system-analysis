# Data Architecture

> Ownership, source-of-truth boundaries, isolation, and lifecycle of Aveli data.

## Purpose

`architecture/` defines **where responsibility for data begins and ends before physical schemas are considered**.

## Responsibility

This area owns:
- data-domain boundaries;
- canonical data authority;
- user-workspace isolation;
- persistence continuity across logout and access changes;
- lifecycle rules shared by several persistence components.

## Boundaries

Do not define table columns, SQL types, ORM models, indexes, or migration syntax here.

## Navigation

- [`data-ownership.md`](data-ownership.md)
- [`data-lifecycle.md`](data-lifecycle.md)

Canonical business rules:
[`../../business/requirements/business-rules.md`](../../business/requirements/business-rules.md)
