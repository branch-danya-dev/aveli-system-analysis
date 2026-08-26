# Aveli — Security

## Purpose

This document describes the main security boundaries and controls used in Aveli.

The focus is on protecting account credentials, access state, billing integration and local workspace isolation.

---

## Security Model

Aveli separates security responsibilities into three areas:

```text
Identity
   ↓
Authentication / Session Security

Access
   ↓
Entitlement / Billing Security

Workspace
   ↓
Local Data Isolation

Each area has different trust boundaries.

Authentication Security

The backend is responsible for user identity and session validation.

Current model:

Credentials
   ↓
Backend Validation
   ↓
JWT Access Token
   +
Rotating Refresh Token

Passwords are not stored in plain text.

The backend uses a strong password hashing mechanism.

Token Storage

Authentication tokens are stored in secure device storage.

Access Token
Refresh Token
      ↓
Secure Storage

Tokens must not be stored in:

plain application preferences;
local workspace tables;
log output;
source-controlled configuration.
Refresh Token Rotation

Refresh tokens are rotated during session renewal.

Conceptually:

Old Refresh Token
      ↓
Validate
      ↓
Invalidate / Replace
      ↓
New Refresh Token

This reduces the lifetime and reuse potential of previously issued refresh credentials.

Access Snapshot Security

Aveli persists a trusted access snapshot to support temporary offline operation.

The snapshot is stored in secure device storage.

It may include:

current access state;
effective source;
verification timestamp;
next required verification time.

The cached snapshot is not the permanent source of truth.

Its validity is limited by the offline verification policy.

Local Workspace Isolation

Operational data is isolated by authenticated user.

User A
  ↓
aveli_<userA>.sqlite

User B
  ↓
aveli_<userB>.sqlite

Visit photos follow the same ownership boundary.

A session change must therefore include:

Close previous DB
      ↓
Resolve authenticated user
      ↓
Open correct user DB

This prevents accidental cross-account data exposure.

Logout Security

Logout must clear active identity state.

Expected behavior:

Logout
  ↓
Clear tokens
  ↓
Close user database
  ↓
Cancel user-specific reminders

Persistent workspace data remains on the device.

This is intentional: logout removes access to the active session, not ownership of stored work.

API Security

Production backend communication must use:

Mobile Client
     ↓
   HTTPS
     ↓
Aveli Backend

Authenticated endpoints require valid session credentials.

The client must not treat local UI state as proof of server authorization.

Secret Management

Aveli distinguishes public mobile configuration from backend secrets.

Allowed in Mobile Client

Examples:

public RevenueCat SDK key;
public application configuration;
API endpoint.
Backend Only

Examples:

RevenueCat secret credentials;
webhook authentication secrets;
database credentials;
server signing secrets.
Public SDK Key
     ↓
Mobile App

Secret Credentials
     ↓
Backend Environment
RevenueCat Security Boundary

The mobile application communicates with RevenueCat using the supported public SDK configuration.

Sensitive verification remains server-side.

Mobile App
   ↓
RevenueCat Public SDK

RevenueCat
   ↓
Authenticated Backend Integration
   ↓
Aveli Backend

A client-side purchase result alone must not become an unrestricted trust boundary.

Webhook Security

RevenueCat webhooks are server-to-server inputs.

The backend must validate incoming webhook requests according to the configured integration security mechanism before modifying subscription state.

Invalid or unauthenticated events must not change access state.

Data Privacy

The current architecture deliberately avoids uploading operational workspace data to the Aveli backend.

The backend does not normally receive:

client lists;
appointments;
visit notes;
visit photos;
local payments.

This reduces the amount of sensitive professional data exposed to server-side infrastructure.

Local Data Risks

Because workspace data is device-owned, some security responsibilities remain with the device environment.

Potential risks include:

compromised device;
operating-system-level storage access;
device loss;
manual removal of local application data.

The current architecture does not provide server-side recovery of workspace data because cloud synchronization is outside the current scope.

Notification Privacy

Local notifications may contain appointment-related information.

When a user logs out:

Logout
   ↓
Cancel account-specific notifications

This prevents appointment information from the previous account from appearing after account switching.

Error Handling

Security-sensitive failures should not expose internal implementation details.

Examples:

Authentication failure
→ user-safe error

Invalid entitlement
→ Access Gate

Internal backend failure
→ generic recoverable error

Raw secrets, tokens or internal stack information must not be presented to the user.

Production Constraints

Security depends partly on release configuration.

Production builds must not contain:

development API addresses;
loopback endpoints;
standalone bypass mode;
backend secrets;
debug-only authentication behavior.

Release configuration is therefore part of the security model rather than only deployment convenience.

Trust Boundaries

The main trust boundaries are:

User Device
   │
   ├── Local Workspace
   ├── Secure Storage
   │
   └──── HTTPS ────► Aveli Backend
                         │
                         ├── PostgreSQL
                         │
                         └────► RevenueCat

Crossing each boundary requires explicit validation.

Security Principles

Aveli follows several core principles:

Identity is validated by the backend.
Sensitive tokens are stored securely on the device.
Backend secrets never belong in the mobile client.
Operational data is isolated per user.
Cached access is temporary trust, not permanent authority.
Billing state is verified through controlled integration boundaries.
Summary

Aveli security is built around separation of responsibility:

Backend
→ identity and entitlement trust

Secure Storage
→ session and cached access state

Local Workspace
→ operational data ownership

The architecture minimizes server exposure of user work data while keeping account, billing and access decisions under server control.