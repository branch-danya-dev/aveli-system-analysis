# Aveli — Authentication

## Purpose

This document describes how user authentication works in Aveli and how account identity is connected to the local workspace.

Authentication is responsible for identifying the user and establishing a trusted session with the backend.

---

## Authentication Scope

Authentication covers:

- account registration;
- sign-in;
- session restoration;
- access token handling;
- refresh token lifecycle;
- logout.

Authentication does not store or synchronize the user's operational workspace data.

---

## Registration Flow

For a new user:

```text
Open Aveli
   ↓
Register account
   ↓
Send credentials to backend
   ↓
Create account
   ↓
Create 30-day trial
   ↓
Create authenticated session
   ↓
Return access / refresh tokens
   ↓
Open local user workspace

The trial is created as part of the server-side registration flow.

Sign-In Flow

For an existing user:

Enter credentials
   ↓
Send sign-in request
   ↓
Backend validates account
   ↓
Credentials valid?
   /          \
 Yes          No
  ↓            ↓
Create      Reject
session     request
  ↓
Return tokens
  ↓
Resolve user workspace

Invalid credentials must not create an authenticated session.

Session Model

Aveli uses two token types:

Access Token
    +
Refresh Token
Access Token

Used for authenticated API requests.

It is expected to have a limited lifetime.

Refresh Token

Used to obtain a new authenticated access session without requiring the user to enter credentials again.

Refresh tokens are rotated by the backend.

Token Storage

Authentication tokens are stored using secure device storage.

Backend
   ↓
Access + Refresh Tokens
   ↓
Secure Storage

Tokens must not be stored inside the local workspace database.

This keeps account credentials separate from operational user data.

Session Restoration

On application startup:

Launch
   ↓
Check stored session
   ↓
Session available?
   /          \
 Yes          No
  ↓            ↓
Restore      Auth screen
identity
  ↓
Open user DB
  ↓
Resolve access state

Session restoration is part of the application bootstrap process.

User-to-Workspace Mapping

After authentication, the account identity determines which local database is opened.

Authenticated User
       ↓
     userId
       ↓
aveli_<userId>.sqlite

This creates a direct boundary between authenticated identity and local workspace ownership.

A different user must not inherit or see another user's workspace.

Refresh Flow

When the access token is no longer valid:

Authenticated API request
        ↓
Access token expired
        ↓
Use refresh token
        ↓
Backend validates refresh session
        ↓
Rotate refresh token
        ↓
Issue new access token
        ↓
Continue session

If refresh validation fails, the session can no longer be trusted.

The user must authenticate again.

Logout

Logout performs identity cleanup:

Logout
  ↓
Clear active tokens
  ↓
Close active local database
  ↓
Cancel user-specific reminders
  ↓
Return to auth flow

Logout does not delete:

local workspace database;
visit photos;
historical operational data.

Authentication state and workspace persistence are intentionally separated.

Account Switching

When one user logs out and another logs in:

User A
  ↓
Logout
  ↓
Close User A DB
  ↓
Clear User A session
  ↓
User B signs in
  ↓
Open User B DB

At no point should User A's workspace appear inside User B's session.

Failure Cases

Authentication must handle:

invalid credentials;
expired access token;
invalid refresh token;
network failure;
deleted or unavailable account;
corrupted or missing local token state.

Authentication failures must not delete local workspace data.

Security Boundary
Authentication
      ↓
Identity
      ↓
Session

Authentication establishes who the user is.

Access logic separately determines whether that user is currently allowed to enter the workspace.

These are related but distinct concerns.

Summary

Aveli authentication follows this model:

Credentials
    ↓
Backend Identity
    ↓
Session Tokens
    ↓
Authenticated User
    ↓
Local User Workspace

The backend owns identity and session state, while the device owns the user's operational workspace.