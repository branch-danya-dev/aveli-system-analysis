<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=220&text=Aveli&fontAlign=50&fontAlignY=40&desc=Offline-first%20mobile%20workspace%20%C2%B7%20System%20Analysis%20Case&descAlign=50&descAlignY=60&animation=fadeIn" alt="Aveli banner" />
</p>

<p align="center">
  <strong>Mobile workspace for independent beauty professionals with local-first data, server-managed access and subscription billing.</strong>
</p>

<p align="center">
  <code>System Analysis</code>
  <code>Mobile Architecture</code>
  <code>Offline-first</code>
  <code>REST API</code>
  <code>Data Ownership</code>
  <code>Billing Integration</code>
</p>

---

## What is Aveli?

**Aveli** is a mobile daily workspace for independent beauty professionals.

It helps manage:

- appointments and daily schedule;
- clients and visit history;
- services and pricing;
- payments and outstanding balances;
- visit notes and photos;
- reminders and profile settings.

The product is intentionally designed as a lightweight alternative to a heavy CRM.

Its main architectural idea is simple:

```text
Operational Work Data
        ↓
     Device

Identity / Access / Billing
        ↓
     Backend
```

Client, appointment and payment data stay on the device, while the backend manages identity, trial, subscription and entitlement.

---

## Product Screens

<table>
<tr>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-today-screen.png" alt="Aveli Today" />
  <br>
  <sub><b>Today</b> — daily schedule and workspace overview</sub>
</td>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-calendar-screen.png" alt="Aveli Calendar" />
  <br>
  <sub><b>Calendar</b> — appointments and day planning</sub>
</td>
</tr>
<tr>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-clients-screen.png" alt="Aveli Clients" />
  <br>
  <sub><b>Clients</b> — directory and client history</sub>
</td>
<td width="50%" align="center">
  <img src="11-Result/screenshots/aveli-another-screen.png" alt="Aveli Screen" />
  <br>
  <sub><b>Workspace</b> — supporting application flow</sub>
</td>
</tr>
</table>

---

## The System Problem

Aveli has two very different concerns.

The first is the user's actual work:

```text
Clients
Appointments
Services
Payments
Notes
Photos
Schedule
```

The second is commercial and account infrastructure:

```text
Authentication
Trial
Subscription
Access Grants
Billing
```

Mixing these concerns would make everyday work dependent on network availability and would move sensitive client data into backend infrastructure unnecessarily.

So the solution was designed around a strict system boundary.

---

## Solution

<table>
<tr>
<td width="50%" valign="top">

### Local Workspace

Stored on the user's device:

- Clients
- Appointments
- Services
- Payments
- Visit notes
- Visit photos
- Schedule
- Local settings

**Storage:** Drift / SQLite + device files

</td>
<td width="50%" valign="top">

### Account & Access

Managed by the backend:

- User accounts
- Sessions
- Trial state
- Manual grants
- Lifetime grants
- Subscription state
- Billing synchronization

**Storage:** PostgreSQL

</td>
</tr>
</table>

The backend decides **whether the workspace can be opened**.

It does not become the source of truth for the user's professional data.

---

## High-Level Architecture

<p align="center">
  <img src="renderer/system-context.svg" alt="Aveli System Context" width="900" />
</p>

```text
Flutter Application
        │
        ├── Local Workspace ──────→ Drift / SQLite
        │                           Local Files
        │
        └── Account & Access ─────→ NestJS API
                                      │
                                      ├── PostgreSQL
                                      │
                                      └── RevenueCat
                                             │
                                   Google Play / App Store
```

---

## Key System Decisions

### 01 · Local-first workspace

Daily work remains available without permanent backend connectivity.

The application reads and writes operational data directly from local storage.

### 02 · Per-user data isolation

Each authenticated account has its own local workspace database:

```text
aveli_<userId>.sqlite
```

Visit photos follow the same user-specific ownership model.

### 03 · Server-controlled trial

The 30-day trial is created on the backend.

That means:

```text
Logout      ─┐
Reinstall   ├─→ does not reset trial
Clear data  ─┘
```

The server account remains the source of truth.

### 04 · Unified Access Gate

The workspace is either available or blocked as a whole.

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

The application does not scatter individual premium checks across screens.

### 05 · Access does not own data

Expiration of trial or subscription blocks workspace availability.

It does **not** delete:

- clients;
- appointments;
- payments;
- notes;
- photos.

When access is restored, the same local workspace becomes available again.

### 06 · Controlled offline access

A trusted access snapshot is stored in secure storage.

This allows temporary offline operation while preserving server authority over entitlement.

```text
Last verified access
        ↓
Secure snapshot
        ↓
Offline grace
        ↓
Verification required
```

### 07 · Billing behind an entitlement model

Aveli does not base access directly on a client-side `isPremium` flag.

```text
Google Play / App Store
          ↓
      RevenueCat
          ↓
     Aveli Backend
          ↓
     Access Decision
```

Monthly and yearly plans map to one logical entitlement:

```text
support
```

---

## Access State Model

<p align="center">
  <img src="renderer/access-state-machine.svg" alt="Aveli Access State Machine" width="900" />
</p>

The access layer resolves one effective source and then decides whether the workspace can open.

This keeps trial, subscription and manual grants inside one deterministic model.

---

## Data Model

<p align="center">
  <img src="renderer/data-model.svg" alt="Aveli Data Model" width="900" />
</p>

The important distinction is not only entity relationships, but **ownership**:

```text
SERVER DOMAIN
Account
Session
Access
Subscription

        │ controls availability
        ▼

LOCAL DOMAIN
Client
Service
Appointment
Payment
Visit Notes
Visit Photos
Schedule
```

---

## Integration Flow

<p align="center">
  <img src="renderer/integration-sequence.svg" alt="Aveli Integration Sequence" width="900" />
</p>

The main integration surface includes:

- authentication;
- access resolution;
- billing synchronization;
- RevenueCat webhooks;
- purchase restore;
- offline fallback.

Key backend endpoints:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
```

---

## Development Flow

The product was developed from system boundaries inward rather than from isolated screens.

```text
Product idea
    ↓
User workflows
    ↓
Domain model
    ↓
Data ownership
    ↓
Authentication & access
    ↓
External integrations
    ↓
Offline behavior
    ↓
Security & release constraints
    ↓
Implementation
    ↓
Automated verification
```

The most important decisions were made around the points where different concerns meet:

- local data vs backend data;
- authentication vs entitlement;
- subscription state vs workspace access;
- cached access vs server authority;
- development flexibility vs production safety.

---

## System Analysis Artifacts

| Area | Documentation |
|---|---|
| Context & Scope | [`01-Context-and-Scope`](01-Context-and-Scope/) |
| User Journey | [`02-User-Journey`](02-User-Journey/) |
| Requirements | [`03-Requirements`](03-Requirements/) |
| System Design | [`04-System-Design`](04-System-Design/) |
| Data & Domain | [`05-Data-and-Domain`](05-Data-and-Domain/) |
| Auth & Access | [`06-Auth-and-Access`](06-Auth-and-Access/) |
| Integrations | [`07-Integrations`](07-Integrations/) |
| Offline & Errors | [`08-Offline-and-Error-Handling`](08-Offline-and-Error-Handling/) |
| Security & Release | [`09-Security-and-Release`](09-Security-and-Release/) |
| Traceability | [`10-Traceability`](10-Traceability/) |
| Result | [`11-Result`](11-Result/) |

---

## Diagram Set

The repository contains rendered diagrams for fast review:

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

Source `.puml` files remain next to the corresponding analytical documents.

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
  <img src="https://img.shields.io/badge/JWT-000000?style=flat-square&logo=jsonwebtokens&logoColor=white">
  <img src="https://img.shields.io/badge/RevenueCat-F25A5A?style=flat-square">
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
- subscription flows;
- appointment rules;
- payment rules;
- database migrations;
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

Aveli demonstrates analysis of a system where mobile UX, local persistence, backend identity, billing and offline behavior must remain consistent with each other.

The case covers:

```text
Requirements
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

The result is a real implemented product with explicit system boundaries and traceable design decisions.

---

<p align="center">
  <strong>System analysis designed to survive implementation.</strong>
</p>
