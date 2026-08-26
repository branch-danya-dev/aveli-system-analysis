# Aveli — Outcome

## Purpose

This document summarizes what Aveli represents as a completed system-analysis case.

The focus is not only on implemented features, but on the system decisions that shape the product.

---

## Result

Aveli became a mobile workspace that combines:

- client management;
- appointment scheduling;
- services and pricing;
- payments;
- visit notes and photos;
- local reminders;
- account authentication;
- server-controlled trial;
- subscription access;
- offline workspace operation.

The product is designed as a lightweight alternative to a traditional CRM for independent beauty professionals.

---

## Delivered System Model

The final solution is built around two clearly separated domains:

```text
Operational Workspace
        ↓
Local Device

Identity / Access / Billing
        ↓
Backend

This separation became the central architectural principle of the system.

Operational data such as clients, appointments and payments remains local, while the backend owns account, session, trial and subscription state.

Key System Decisions
1. Local-first workspace

Daily work does not depend on constant backend availability.

The user's operational data remains in a per-user SQLite database and local file storage.

2. Server-controlled access

Trial and entitlement state are controlled by the backend.

A reinstall or local database reset cannot create a new trial for the same account.

3. Unified Access Gate

The workspace is either available or unavailable as a whole.

Aveli does not spread independent premium checks across individual features.

This keeps access behavior deterministic.

4. Access does not equal ownership

Expiration of trial or subscription blocks workspace access but does not delete local work data.

When valid access is restored, the existing workspace becomes available again.

5. Controlled offline behavior

A persisted access snapshot allows temporary offline work.

Cached entitlement remains valid only within the configured verification policy rather than becoming permanent offline authorization.

6. External billing isolated behind entitlement

RevenueCat and the mobile stores manage subscription lifecycle.

The Aveli backend converts that external state into the application's own access model.

Store
  ↓
RevenueCat
  ↓
Backend
  ↓
Access Decision
System Coverage

The analysis covers the system from user interaction to production constraints:

User Journey
     ↓
Requirements
     ↓
Business Rules
     ↓
Domain & Data
     ↓
Architecture
     ↓
API & Integrations
     ↓
Authentication & Access
     ↓
Offline & Errors
     ↓
Security
     ↓
Release
Implemented Product Areas

The current application contains dedicated functionality for:

Today;
Calendar;
Clients;
Appointments;
Services;
Payments;
Settings and Profile;
Authentication;
Subscription and Access;
Reminders.

The codebase also contains separate backend modules for authentication, access and billing.

Technical Context
Area	Technology
Mobile	Flutter
State / DI	Riverpod
Local data	Drift / SQLite
Backend	NestJS
ORM	Prisma
Server database	PostgreSQL
Authentication	JWT + rotating refresh
Billing	RevenueCat
Secure storage	flutter_secure_storage
Platforms	Android / iOS

The current implementation also includes automated coverage for access logic, authentication, database migrations, appointments, payments and other domain behavior.

What This Case Demonstrates

Aveli demonstrates system analysis across several different concerns:

defining system boundaries;
separating data ownership;
modeling domain entities;
defining business rules;
designing authentication and entitlement flows;
integrating external billing;
modeling offline behavior;
defining API responsibilities;
handling failure scenarios;
designing release constraints;
maintaining traceability from rules to acceptance criteria.
Screenshots

Recommended screenshots for the portfolio:

screenshots/
├── today.png
├── calendar.png
├── clients.png
├── appointment.png
└── profile.png

The screenshots should demonstrate that the described system exists as an implemented product rather than only as analytical documentation.

Final Architecture
                 Aveli
                   │
          ┌────────┴────────┐
          │                 │
      Workspace        Account / Access
          │                 │
   SQLite / Files         Backend
                            │
                       PostgreSQL
                            │
                       RevenueCat
                            │
                  Google Play / App Store
Summary

Aveli is a useful system-analysis case because it combines a real mobile product with several non-trivial system boundaries:

local data
+
server identity
+
entitlement
+
billing integration
+
offline behavior
+
production constraints

The result is not only a mobile application, but a complete system with explicit ownership, access, integration and lifecycle rules.