<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=220&text=Aveli&fontAlign=50&fontAlignY=38&desc=System%20Analysis%20Case%20%C2%B7%20Offline-first%20Mobile%20Workspace&descAlign=50&descAlignY=58&animation=fadeIn" alt="Aveli banner" />
</p>

<p align="center">
  <strong>Offline-first mobile workspace for independent beauty professionals.</strong>
</p>

<p align="center">
  <code>System Analysis</code>
  <code>Mobile Architecture</code>
  <code>Data Ownership</code>
  <code>REST API</code>
  <code>Offline-first</code>
  <code>Billing</code>
</p>

---

## Overview

**Aveli** is a mobile workspace for independent beauty professionals who manage appointments, clients, services, payments, notes and visit photos.

The system is intentionally split into two responsibility zones:

```text
Operational Workspace
        ↓
   Local Device

Identity / Access / Billing
        ↓
      Backend
```

This boundary keeps day-to-day work local while allowing the backend to manage authentication, trial periods, access grants and subscription state.

---

## Product Screens

<table>
<tr>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-today-screen.png" alt="Aveli Today" width="92%" />
  <br>
  <sub><b>Today</b> — daily schedule and current workload</sub>
</td>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-calendar-screen.png" alt="Aveli Calendar" width="92%" />
  <br>
  <sub><b>Calendar</b> — day and week planning</sub>
</td>
</tr>
<tr>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-clients-screen.png" alt="Aveli Clients" width="92%" />
  <br>
  <sub><b>Clients</b> — local client directory and visit history</sub>
</td>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-another-screen.png" alt="Aveli Additional Screen" width="92%" />
  <br>
  <sub><b>Workspace</b> — supporting product flow</sub>
</td>
</tr>
</table>

---

## Problem

Aveli combines two different system concerns.

The first is the user's actual operational work:

```text
Clients
Appointments
Services
Payments
Visit Notes
Visit Photos
Schedule
```

The second is account and commercial infrastructure:

```text
Authentication
Trial
Subscription
Access Grants
Billing
```

If these concerns were mixed, everyday work would become dependent on network availability and sensitive operational data would unnecessarily move into backend infrastructure.

The architecture therefore treats **data ownership and access control as separate problems**.

---

## Solution

<table>
<tr>
<td width="50%" valign="top">

### Local Workspace

Owned by the mobile application:

- clients;
- appointments;
- services;
- payments;
- visit notes;
- visit photos;
- schedule;
- local settings.

**Storage:** Drift / SQLite + local device files

</td>
<td width="50%" valign="top">

### Account & Access

Owned by the backend:

- user accounts;
- sessions;
- trial state;
- manual grants;
- lifetime grants;
- subscriptions;
- billing synchronization.

**Storage:** PostgreSQL

</td>
</tr>
</table>

The backend answers:

```text
Who is the user?
Can this user open the workspace?
```

It does not answer:

```text
What clients, appointments or payments does the user have?
```

---

## Architecture

<p align="center">
  <img src="renderer/system-context.svg" alt="Aveli System Context" width="900" />
</p>

```text
Flutter App
   │
   ├── Local Workspace ──→ Drift / SQLite
   │                       Local Files
   │
   └── Account & Access ─→ NestJS API
                              │
                              ├── PostgreSQL
                              │
                              └── RevenueCat
                                     │
                          Google Play / App Store
```

---

## Key System Decisions

### 01 · Local-first data ownership

Operational data stays on the user's device.

This allows the workspace to remain usable without continuous backend connectivity and keeps client data outside the server-side account infrastructure.

### 02 · Per-user workspace isolation

Each authenticated account opens its own local database:

```text
aveli_<userId>.sqlite
```

Visit photos follow the same per-user storage boundary.

### 03 · Server-controlled trial

The 30-day trial is created and owned by the backend.

```text
Logout
Reinstall
Clear local data
        ↓
Does not create a new trial
```

### 04 · Unified access model

Workspace access is resolved through one ordered source model:

```text
Lifetime
   ↓
Manual Grant
   ↓
Subscription
   ↓
Trial
   ↓
None
```

The app does not scatter feature-level premium checks across the UI.

### 05 · Access does not own data

If access expires, the workspace is blocked but not destroyed.

```text
Access expired
      ↓
Access Gate
      ↓
Local data remains
```

Once access is restored, the same local workspace becomes available again.

### 06 · Controlled offline grace

The client stores a trusted access snapshot in secure storage.

This supports temporary offline use while preserving the backend as the authority for entitlement.

### 07 · Billing behind entitlement

A store purchase is not treated as a direct `isPremium = true` switch.

```text
Store
  ↓
RevenueCat
  ↓
Aveli Backend
  ↓
Access Decision
```

Monthly and yearly products both map to the logical entitlement:

```text
support
```

---

## Access State

<p align="center">
  <img src="renderer/access-state-machine.svg" alt="Aveli Access State Machine" width="900" />
</p>

The state machine shows how lifetime, manual grants, subscriptions and trial access are resolved into one effective workspace decision.

---

## Data Model

<p align="center">
  <img src="renderer/data-model.svg" alt="Aveli High-Level Data Model" width="900" />
</p>

The central design distinction is ownership:

```text
SERVER DOMAIN

Account
Session
AccessGrant
Subscription

        │
        │ controls availability
        ▼

LOCAL DOMAIN

Client
Service
Appointment
Payment
VisitNote
VisitPhoto
Settings
```

---

## Integration Sequence

<p align="center">
  <img src="renderer/integration-sequence.svg" alt="Aveli Integration Sequence" width="900" />
</p>

The main integration flow covers:

- authentication;
- access resolution;
- purchase and restore;
- billing synchronization;
- RevenueCat webhooks;
- offline fallback.

Key API boundaries:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
```

---

## Development Approach

The system evolved from boundary decisions toward implementation.

```text
Product Idea
    ↓
User Journeys
    ↓
Requirements
    ↓
Business Rules
    ↓
Domain Model
    ↓
Data Ownership
    ↓
System Architecture
    ↓
Auth & Access
    ↓
External Integrations
    ↓
Offline Behavior
    ↓
Security & Release Constraints
    ↓
Implementation
    ↓
Automated Verification
```

The most important design work happened at the boundaries between concerns:

- local data vs server data;
- authentication vs entitlement;
- subscription state vs workspace access;
- cached access vs server authority;
- development configuration vs production safety.

---

## Documentation

| Section | Contents |
|---|---|
| [`01-Context-and-Scope`](01-Context-and-Scope/) | Product context and system boundary |
| [`02-User-Journey`](02-User-Journey/) | Main and access journeys |
| [`03-Requirements`](03-Requirements/) | FR, NFR, business rules, acceptance criteria |
| [`04-System-Design`](04-System-Design/) | Solution overview and architecture |
| [`05-Data-and-Domain`](05-Data-and-Domain/) | Domain model and data ownership |
| [`06-Auth-and-Access`](06-Auth-and-Access/) | Authentication, access model and state |
| [`07-Integrations`](07-Integrations/) | API boundaries and RevenueCat |
| [`08-Offline-and-Error-Handling`](08-Offline-and-Error-Handling/) | Offline behavior and edge cases |
| [`09-Security-and-Release`](09-Security-and-Release/) | Security and release constraints |
| [`10-Traceability`](10-Traceability/) | Rule → requirement → acceptance mapping |
| [`11-Result`](11-Result/) | Outcome and product screenshots |

---

## Diagram Set

```text
renderer/
├── user-flow.svg
├── system-context.svg
├── component-model.svg
├── data-model.svg
├── access-sequence.svg
├── access-state-machine.svg
└── integration-sequence.svg
```

PlantUML source files remain next to the corresponding analytical documents.

---

## Technology Stack

<p align="center">
  <img src="https://img.shields.io/badge/Flutter-02569B?style=flat-square&logo=flutter&logoColor=white">
  <img src="https://img.shields.io/badge/Dart-0175C2?style=flat-square&logo=dart&logoColor=white">
  <img src="https://img.shields.io/badge/Riverpod-0D1117?style=flat-square">
  <img src="https://img.shields.io/badge/SQLite-003B57?style=flat-square&logo=sqlite&logoColor=white">
  <img src="https://img.shields.io/badge/NestJS-E0234E?style=flat-square&logo=nestjs&logoColor=white">
  <img src="https://img.shields.io/badge/Prisma-2D3748?style=flat-square&logo=prisma&logoColor=white">
  <img src="https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white">
  <img src="https://img.shields.io/badge/JWT-000000?style=flat-square">
  <img src="https://img.shields.io/badge/RevenueCat-Billing-0D1117?style=flat-square">
  <img src="https://img.shields.io/badge/PlantUML-Diagrams-0D1117?style=flat-square">
</p>

### Client

```text
Flutter
Riverpod
go_router
Drift / SQLite
flutter_secure_storage
purchases_flutter
flutter_local_notifications
```

### Backend

```text
NestJS
Prisma
PostgreSQL
Argon2id
JWT access + rotating refresh
RevenueCat REST + webhooks
```

---

## Quality & Release Safety

The project includes automated verification around:

- authentication and session lifecycle;
- access state;
- subscription behavior;
- appointment rules;
- payment rules;
- local database migrations;
- offline access;
- release configuration.

Production builds explicitly reject unsafe development configuration such as:

```text
localhost
127.0.0.1
10.0.2.2
AVELI_STANDALONE=true
```

---

## Outcome

Aveli demonstrates system analysis of a real mobile product where local persistence, backend identity, subscription billing and offline behavior must remain consistent with each other.

The case covers:

```text
Requirements
+
Business Rules
+
Domain Modeling
+
Data Ownership
+
Mobile Architecture
+
REST API
+
Authentication
+
Entitlement
+
Billing Integration
+
Offline Strategy
+
Security
+
Release Constraints
```

<p align="center">
  <strong>System analysis designed to survive implementation.</strong>
</p>
