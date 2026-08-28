<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&height=220&text=Aveli&fontAlign=50&fontAlignY=38&desc=System%20Analysis%20Case%20%C2%B7%20Local-first%20Mobile%20Workspace&descAlign=50&descAlignY=58&animation=fadeIn&color=gradient&customColorList=12,14,19,20,24&fontColor=fff7f2&descColor=fff7f2" alt="Aveli banner" />
</p>

<p align="center">
  <a href="README.md"><b>English</b></a> ·
  <a href="README.ru.md">Русский</a>
</p>

<p align="center">
  <strong>System-analysis case for a local-first mobile workspace with server-controlled identity, access and subscription billing.</strong>
</p>

<p align="center">
  <code>System Analysis</code>
  <code>Local-first</code>
  <code>Data Ownership</code>
  <code>REST API</code>
  <code>Offline Access</code>
  <code>Billing Integration</code>
  <code>SSAD</code>
</p>

---

## What is Aveli?

**Aveli** is a personal mobile workspace for independent beauty professionals.

It supports day-to-day work with:

- clients and visit history;
- appointments and calendar;
- services and prices;
- payments and outstanding balances;
- visit notes and photos;
- local reminders;
- profile and workspace settings.

The product is intentionally lightweight. It is not designed as a salon-management platform or a server-hosted CRM.

The defining architectural split is:

```text
Professional Workspace
        ↓
   User Device

Identity / Access / Billing
        ↓
      Backend
```

Professional data remain local. Account identity, sessions, trial, grants and normalized subscription state remain server-controlled.

---

## Product Screens

<table>
<tr>
<td width="50%" align="center">
  <img src="screenshots/aveli-today-screen.png" alt="Aveli Today" />
  <br><sub><b>Today</b> — current schedule and daily workspace</sub>
</td>
<td width="50%" align="center">
  <img src="screenshots/aveli-calendar-screen.png" alt="Aveli Calendar" />
  <br><sub><b>Calendar</b> — appointments and day planning</sub>
</td>
</tr>
<tr>
<td width="50%" align="center">
  <img src="screenshots/aveli-clients-screen.png" alt="Aveli Clients" />
  <br><sub><b>Clients</b> — client directory and history</sub>
</td>
<td width="50%" align="center">
  <img src="screenshots/aveli-another-screen.png" alt="Aveli Workspace" />
  <br><sub><b>Workspace</b> — supporting product flow</sub>
</td>
</tr>
</table>

---

## System Problem

Aveli combines two concerns with very different ownership and availability requirements.

### Professional work

```text
Clients
Services
Appointments
Payments
Visit notes
Visit photos
Schedule
Workspace settings
```

These data are operational and useful even when the backend is temporarily unavailable.

### Account and commercial infrastructure

```text
Authentication
Sessions
Registration trial
Manual / lifetime grants
Subscription
Billing reconciliation
Workspace access
```

These require a trusted server-side authority.

The current architecture therefore avoids making professional work a synchronized backend domain unless the product boundary changes in the future.

---

## High-Level Architecture

<p align="center">
  <img src="renderer/system-context.svg" alt="Aveli System Context" width="900" />
</p>

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

Supporting external boundaries include device contacts, local notifications, camera/gallery, OS handoff and an exchange-rate API.

The full cross-layer view lives in [`system/`](system/).

---

## Core System Decisions

### 01 · Local-first workspace

Normal professional operations read and write local persistence directly.

```text
User action
   ↓
Frontend domain/repository
   ↓
Drift / SQLite + local files
```

No cloud copy of clients, appointments, payments, notes or photos is required for ordinary work.

### 02 · Per-user workspace isolation

Authenticated backend identity selects the local workspace:

```text
users.id
   ↓
aveli_<userId>.sqlite
```

Visit photos and the cached access snapshot follow the same user-specific boundary.

### 03 · Backend-controlled access

The backend resolves one effective source:

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

The frontend consumes the resolved result rather than independently calculating entitlement precedence.

### 04 · Access does not own data

```text
Access expired
      ≠
Delete workspace
```

Loss of trial/subscription access can block workspace availability, but it does not delete local professional data.

### 05 · Controlled offline trust

A previously verified access state is stored in secure storage.

```text
Backend verification
      ↓
Trusted snapshot
      ↓
Temporary offline authorization
      ↓
Verification required again
```

The server-provided verification deadline is preferred. The current client also contains a 72-hour implementation default when applicable.

### 06 · Purchase is not direct workspace access

```text
Store purchase
      ↓
RevenueCat
      ↓
POST /v1/billing/sync
      ↓
Backend reconciliation
      ↓
AccessStatusView
      ↓
Frontend Access Gate
```

A client-side RevenueCat result does not bypass the Aveli backend access model.

### 07 · Logout is not profile deletion

Logout clears the active identity/access context and closes the local database while preserving the professional workspace.

Explicit profile deletion is a separate destructive lifecycle.

---

## Documentation Architecture

This repository is organized using the working methodology **System-Structured Analysis Documentation (SSAD)**.

> **Documentation mirrors the system.**

Knowledge is owned by the part of the system responsible for it:

```text
business/
database/
backend/
frontend/
integrations/
system/
```

Supporting repository areas:

```text
screenshots/
renderer/
methodology.md
rules.md
```

| Area | Canonical responsibility |
|---|---|
| [`business/`](business/) | Product context, scope, requirements, rules, processes and traceability. |
| [`database/`](database/) | Data ownership, conceptual/logical models and physical persistence. |
| [`backend/`](backend/) | Account, authentication, access, billing, API and server behavior. |
| [`frontend/`](frontend/) | Flutter runtime, navigation, state, local workspace, offline behavior and device usage. |
| [`integrations/`](integrations/) | RevenueCat, stores, device capabilities and third-party boundaries. |
| [`system/`](system/) | Cross-component context, flows, trust, invariants, evolution and review. |
| [`methodology.md`](methodology.md) | Why SSAD is structured this way. |
| [`rules.md`](rules.md) | Normative documentation rules. |

> **Storage is hierarchical. Knowledge is graph-based.**

---

## Recommended Reading Paths

### Fast system review

```text
README
  ↓
business/README
  ↓
system/README
  ↓
system/architecture
  ↓
system/flows
  ↓
system/invariants
```

### Key canonical areas

- Product and requirements → [`business/`](business/)
- Data ownership and persistence → [`database/`](database/)
- Backend contracts and access → [`backend/`](backend/)
- Client runtime and local workspace → [`frontend/`](frontend/)
- External providers and device boundaries → [`integrations/`](integrations/)
- Whole-system synthesis → [`system/`](system/)

### Whole-system review

- [`system/trust/`](system/trust/)
- [`system/invariants/`](system/invariants/)
- [`system/evolution/`](system/evolution/)
- [`system/review/failure-scenarios.md`](system/review/failure-scenarios.md)
- [`system/review/release-readiness.md`](system/review/release-readiness.md)
- [`system/review/open-questions.md`](system/review/open-questions.md)

---

## Technology Context

### Mobile

```text
Flutter / Dart
Riverpod
go_router
Drift
SQLite
flutter_secure_storage
purchases_flutter
flutter_local_notifications
```

### Backend

```text
NestJS
Prisma
PostgreSQL
JWT access tokens
rotating refresh tokens
Argon2id
RevenueCat REST + webhooks
```

### External boundaries

```text
RevenueCat
Apple App Store
Google Play
Device Contacts
OS Notifications
Camera / Gallery
Exchange Rate API
OS Share / File Picker / SMS / Browser
```

---

## API Surface

The backend API is intentionally narrow because professional workspace entities are not server-owned.

Key contracts include:

```text
/v1/auth/*
GET  /v1/access
POST /v1/billing/sync
POST /v1/webhooks/revenuecat
/health
/ready
```

Canonical API documentation: [`backend/api/`](backend/api/)

---

## Data Ownership

The important distinction is not only entity relationships, but ownership.

```text
LOCAL PROFESSIONAL WORKSPACE
Client
Service
Appointment
Payment
Visit Note
Visit Photo
Workspace Settings

SERVER IDENTITY / ACCESS
User
Auth Session
Access Grant
Subscription
Subscription Event
```

Canonical data architecture: [`database/`](database/)

---

## Failure and Release Model

Aveli follows a cross-system isolation principle:

```text
Technical Failure
      ≠
Delete User Work
```

Backend outages, billing failures, unavailable stores, denied permissions or exchange-rate failures should remain isolated from unrelated local professional data.

Production readiness is treated as a whole-system concern because mobile configuration, backend secrets, migrations, store products, RevenueCat mapping and the Access Gate must agree.

See:

- [`system/review/failure-scenarios.md`](system/review/failure-scenarios.md)
- [`system/review/release-readiness.md`](system/review/release-readiness.md)

---

## Diagram Set

Rendered diagrams are kept in [`renderer/`](renderer/):

```text
system-context.svg
component-model.svg
data-model.svg
access-sequence.svg
access-state-machine.svg
integration-sequence.svg
user-flow.svg
```

Machine-maintainable diagram sources remain next to analytical documents where applicable.

---

## Methodology

This repository is the first full validation case for the working SSAD approach.

SSAD favors:

```text
system-shaped hierarchy
+
canonical ownership
+
progressive depth
+
contextual usage
+
evidence-first construction
+
explicit technology modeling
+
traceability
+
late system synthesis
```

Read:

- [`methodology.md`](methodology.md)
- [`rules.md`](rules.md)

SSAD is not presented as a replacement for UML, BPMN, C4, ADR, OpenAPI or docs-as-code. It is an evolving way to organize and connect those forms of knowledge around the actual system.

---

## Current Status

The major analytical perspectives have been migrated to the system-shaped structure and reconciled against implementation evidence used during the analysis.

Remaining unresolved or intentionally open items are kept explicitly in:

[`system/review/open-questions.md`](system/review/open-questions.md)

An unknown should remain visible as an open question rather than be silently converted into an architectural assumption.

---

## Outcome

Aveli demonstrates system analysis across:

```text
Business rules
+
Data ownership
+
Local-first architecture
+
REST contracts
+
Authentication and sessions
+
Workspace entitlement
+
External billing
+
Offline trust
+
Device integrations
+
Failure isolation
+
Release readiness
+
Cross-layer synthesis
```

The result is both an implemented product case and a structured technical knowledge base designed to preserve meaning as analysis moves into real code.

---

<p align="center">
  <strong>System analysis designed to survive implementation.</strong>
</p>
