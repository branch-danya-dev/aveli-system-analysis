# Aveli — Data Ownership

## Purpose

This document defines where Aveli data lives, which system component owns it, and which boundaries must not be crossed.

Data ownership is one of the core architectural principles of the product.

---

## Ownership Principle

Aveli separates data into two independent domains:

```text
Account / Access Data
        ↓
      Backend

Operational Work Data
        ↓
   User Device

   These domains serve different responsibilities and must not be mixed.

Server-Owned Data

The backend owns data required to manage identity and entitlement.

Data	Owner	Storage
User account	Backend	PostgreSQL
Authentication session	Backend	PostgreSQL
Trial state	Backend	PostgreSQL
Manual access grants	Backend	PostgreSQL
Lifetime access	Backend	PostgreSQL
Subscription state	Backend	PostgreSQL
Billing synchronization state	Backend	PostgreSQL

The backend is the source of truth for whether the user is allowed to open the workspace.

Device-Owned Data

The mobile application owns the user's operational workspace data.

Data	Owner	Storage
Clients	Mobile workspace	SQLite
Services	Mobile workspace	SQLite
Appointments	Mobile workspace	SQLite
Payments	Mobile workspace	SQLite
Visit notes	Mobile workspace	SQLite
Schedule settings	Mobile workspace	SQLite
Local preferences	Mobile workspace	SQLite
Visit photos	Mobile workspace	Device file storage

This data is not synchronized to the Aveli backend in the current architecture.

Per-User Isolation

Each authenticated account has its own local workspace.

User A
  ↓
aveli_<userA>.sqlite

User B
  ↓
aveli_<userB>.sqlite

The active account determines which local database is opened.

Workspace data from one account must not be visible while another account is active.

Visit photos follow the same isolation principle through a user-specific file path.

Session Data

Authentication credentials are separate from workspace storage.

The client stores:

Access Token
Refresh Token
Cached Access Snapshot
        ↓
Secure Storage

These values control session and access behavior but do not contain the operational workspace itself.

Logout Behavior

Logout affects identity state, not data ownership.

Logout
  ↓
Clear active tokens
  ↓
Close active local database
  ↓
Cancel account-specific reminders

The following remain on the device:

local SQLite database;
visit photos;
workspace history.

This allows the same account to recover its existing workspace after signing in again.

Access Expiration

Loss of entitlement does not change data ownership.

Trial / Subscription expires
          ↓
      Access Gate
          ↓
Local workspace remains unchanged

The system blocks access to the workspace but does not delete the user's operational data.

If access is later restored, the same local workspace becomes available again.

Backend Boundary

The backend currently must not become the source of truth for:

clients;
appointments;
services;
payments;
visit notes;
visit photos;
schedule data.

This means Aveli is not currently a cloud-synchronized CRM.

Why This Boundary Exists

The separation supports several product qualities:

Privacy

Client and visit data do not need to leave the user's device.

Offline Operation

Daily work does not depend on backend availability.

Clear Responsibility

The backend controls access.

The device controls work data.

Failure Isolation

Backend, billing or entitlement problems do not corrupt or remove operational workspace data.

Ownership Matrix
Domain	Mobile App	Backend	External Service
Clients	Owner	—	—
Appointments	Owner	—	—
Services	Owner	—	—
Payments	Owner	—	—
Visit photos	Owner	—	—
Authentication	Client participant	Owner	—
Trial	Consumer	Owner	—
Access grants	Consumer	Owner	—
Subscription	Consumer	Resolved state	RevenueCat / Store
Exchange rates	Consumer	—	External API
Future Impact

Some future features would require revisiting this ownership model.

For example:

Multi-device synchronization
Public online booking
Shared workspace
Cloud backup

These features would require server-side ownership or synchronization of at least part of the operational domain.

Until then, the current boundary remains:

Work Data → Device
Access Data → Backend
Summary

Aveli uses a strict split between operational data and account data.

The mobile device owns the user's day-to-day workspace, while the backend owns identity, trial, subscription and access state.

This separation is the foundation of Aveli's privacy, offline behavior and access model.