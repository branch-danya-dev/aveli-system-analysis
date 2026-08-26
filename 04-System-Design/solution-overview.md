# Aveli — Solution Overview

## Purpose

This document describes the high-level solution structure of Aveli and the main architectural decisions behind it.

The goal is to show how the mobile client, local workspace, backend and billing integration work together.

---

## System Overview

Aveli is built as a hybrid mobile system with two separate responsibility areas:

```text
Aveli Mobile Application
        │
        ├── Local Workspace
        │      ├── Clients
        │      ├── Appointments
        │      ├── Services
        │      ├── Payments
        │      ├── Notes
        │      └── Photos
        │
        └── Account / Access
               ├── Authentication
               ├── Trial
               ├── Subscription
               └── Entitlement

               Operational work data is stored locally.

Account, access and billing state are handled through the backend.

Main Components
User
  │
  ▼
Flutter Client
  │
  ├── Workspace Layer
  │      ↓
  │   Drift / SQLite
  │      +
  │   Local Files
  │
  └── Account & Access Layer
         ↓
       HTTPS
         ↓
      NestJS API
         ↓
      PostgreSQL
         │
         └── RevenueCat
Mobile Client

The Flutter client is responsible for:

application navigation;
user interaction;
local workspace operations;
local database access;
local reminders;
account session handling;
access state presentation;
initiating billing flows;
synchronization of account-related state.

The client follows a layered feature structure:

Presentation
    ↓
Providers / Controllers
    ↓
Domain
    ↓
Repositories / Data Sources

Features are separated by responsibility such as auth, calendar, clients, appointments, payments and subscription.

Local Workspace

The workspace is the operational core of Aveli.

It contains:

clients;
services;
appointments;
visit data;
payments;
schedule;
settings.

The workspace is backed by a per-user SQLite database.

Authenticated User
      ↓
aveli_<userId>.sqlite
      ↓
Workspace Data

Visit photos are stored separately in a user-specific local file tree.

The backend is not the source of truth for this data.

Backend

The backend is responsible for account-level concerns.

Main responsibilities:

registration;
authentication;
session lifecycle;
trial creation;
entitlement resolution;
access grants;
billing synchronization;
RevenueCat webhook processing.

The backend stack consists of:

NestJS
   ↓
Prisma
   ↓
PostgreSQL

The backend does not store normal workspace entities such as clients or appointments.

Access Layer

Workspace availability is resolved through a single access model.

Lifetime
   OR
Manual Grant
   OR
Subscription
   OR
Trial
      ↓
Access Decision
      ↓
Workspace / Access Gate

This decision is made before the user enters the workspace.

A cached trusted access snapshot allows temporary offline operation according to the configured verification policy.

Billing Integration

RevenueCat acts as the billing integration layer between the mobile stores and Aveli.

User
  ↓
Google Play / App Store
  ↓
RevenueCat
  ↓
Aveli Backend
  ↓
Access State

The mobile application initiates purchases and receives customer state.

The backend synchronizes subscription information and resolves the resulting entitlement.

Data Ownership

One of the core design decisions is strict ownership separation:

Account Data
    ↓
Backend

Work Data
    ↓
Device

Server-side data includes:

users;
sessions;
access grants;
subscriptions.

Device-side data includes:

clients;
appointments;
services;
payments;
notes;
photos;
local preferences.

This boundary prevents the account system from becoming the operational data store.

Offline Strategy

Aveli does not require permanent backend availability for daily work.

Backend Available
      ↓
Refresh access if required

Backend Unavailable
      ↓
Use trusted cached access state
      ↓
Continue local work

Workspace operations remain local.

Only account-level operations require backend connectivity.

Startup Flow

At application startup:

Launch
  ↓
Restore Session
  ↓
Open User Database
  ↓
Restore Access Snapshot
  ↓
Verify Access if Required
  ↓
Access Decision
  ↓
Workspace / Access Gate

The workspace is opened only after the active user's identity and access context are resolved.

Key Architectural Decisions
Decision	Reason
Local-first workspace	Keeps daily operations independent from network availability
Per-user SQLite database	Prevents workspace data mixing between accounts
Backend-managed trial	Prevents trial reset through local reinstall
Unified Access Gate	Keeps entitlement behavior consistent
RevenueCat integration	Centralizes store subscription state
Secure cached access snapshot	Supports temporary offline access
Backend excluded from work data	Preserves the product's privacy and local ownership model
Current System Boundary

The current architecture intentionally excludes:

cloud workspace synchronization;
multi-device work data sync;
server-side appointment storage;
public booking backend;
server push notifications.

These may require a different data ownership model if introduced later.

Summary

Aveli is structured around a deliberate separation:

Daily Work
   ↓
Local Device

Identity + Access + Billing
   ↓
Backend

This allows the application to behave as a private offline-oriented workspace while still supporting accounts, trials and store subscriptions.