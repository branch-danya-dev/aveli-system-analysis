# Aveli — API Boundaries

## Purpose

This document defines what crosses the boundary between the Aveli mobile client and backend.

The API is responsible for account, access and billing concerns.

Operational workspace data does not cross this boundary in the current architecture.

---

## API Responsibility

The backend API handles:

- registration;
- sign-in;
- session refresh;
- access resolution;
- trial state;
- manual and lifetime grants;
- subscription synchronization;
- billing webhook processing.

The backend API does not handle:

- clients;
- appointments;
- services;
- payments;
- visit notes;
- visit photos;
- local schedule data.

---

## High-Level Boundary

```text
Mobile Client
    │
    ├── Local Workspace
    │      └── SQLite / Files
    │
    └── Account & Access
           ↓
         HTTPS
           ↓
      Aveli Backend

      Only account-level and entitlement-related data is exchanged with the backend.

Authentication API

Authentication endpoints are grouped under:

/v1/auth/*

Typical responsibilities include:

register;
sign in;
refresh session;
logout / revoke session where supported.

The client sends account credentials and receives authenticated session data.

Operational workspace entities are never part of authentication payloads.

Access API

The main access endpoint is:

GET /v1/access

Purpose:

resolve current effective access state;
determine whether workspace access is allowed;
return the effective access source;
provide validity / verification information where required.

Conceptually:

Request
  ↓
Authenticated Account
  ↓
Backend Access Resolution
  ↓
AccessState

The client consumes the result but does not independently become the source of truth for server-controlled entitlement state.

Billing Sync API

Billing synchronization uses:

POST /v1/billing/sync

The endpoint is responsible for reconciling store / RevenueCat state with the Aveli backend.

Conceptually:

Mobile Purchase State
        ↓
Billing Sync
        ↓
RevenueCat Verification
        ↓
Backend Subscription State
        ↓
Updated Access Decision

The endpoint does not process workspace payments made by the user's own clients.

Aveli subscription billing and local visit payments belong to different domains.

RevenueCat Webhook Boundary

RevenueCat may send subscription lifecycle events to:

POST /v1/webhooks/revenuecat

This boundary is server-to-server.

RevenueCat
    ↓
Webhook
    ↓
Aveli Backend
    ↓
Subscription State
    ↓
Access Resolution

Webhook processing must not depend on the mobile application being open.

Data Crossing the API
Client → Backend

Examples:

registration data;
authentication credentials;
refresh token context;
billing synchronization request;
authenticated access request.
Backend → Client

Examples:

account identity;
session tokens;
access state;
entitlement source;
access validity;
billing synchronization result.
Data That Must Not Cross the API

The current architecture excludes operational workspace payloads such as:

Client records
Appointments
Services
Payments
Visit notes
Visit photos
Schedule

These remain local.

This is a deliberate system boundary, not an implementation limitation.

API Security Boundary

Production communication must follow:

Mobile Client
     ↓
    HTTPS
     ↓
Authenticated API

Security expectations include:

JWT-based authenticated requests;
access / refresh lifecycle;
secure token storage on device;
backend-only secrets;
no RevenueCat secret credentials in the client.
Error Boundary

The client must distinguish between different classes of API failure.

Examples:

Authentication Failure
401 / invalid session
    ↓
Refresh if possible
    ↓
Re-authentication if required
Access Denied
Authenticated
    +
No entitlement
    ↓
Access Gate

This is a valid business state, not necessarily a technical error.

Backend Unavailable
Network / server failure
        ↓
Use cached access if policy allows
        ↓
Preserve local workspace

A backend failure must not be interpreted as a reason to delete local work data.

Boundary Summary
Responsibility	Mobile	Backend
Clients	✓	—
Appointments	✓	—
Services	✓	—
Visit payments	✓	—
Authentication	Client side	Source of truth
Trial	Consumer	Source of truth
Access grants	Consumer	Source of truth
Subscription state	Initiates / displays	Resolves
RevenueCat webhook	—	Handles
Key Principle
API ≠ Workspace Sync

The Aveli API exists to answer:

Who is the user?
        +
Can the user access the workspace?

It does not answer:

What clients and appointments does the user have?

That information remains on the device.

Summary

Aveli keeps the API surface intentionally narrow.

The mobile client owns operational work, while the backend owns identity, entitlement and billing coordination.