# Aveli — RevenueCat Integration

## Purpose

This document describes how Aveli integrates with RevenueCat for subscription purchase, restore and entitlement synchronization.

RevenueCat is used as the store subscription abstraction layer, while the Aveli backend remains responsible for resolving workspace access.

---

## Integration Role

RevenueCat handles:

- store subscription state;
- purchase flow integration;
- restore purchases;
- entitlement mapping;
- subscription lifecycle updates.

Aveli uses one logical entitlement:

```text
support

Both monthly and yearly subscription products grant this entitlement.

Integration Context
User
  ↓
Aveli Mobile App
  ↓
RevenueCat
  ↓
Google Play / App Store

RevenueCat communicates subscription state back to both:

the mobile client;
the Aveli backend.
Purchase Flow
User
  ↓
Open Support Screen
  ↓
Select Monthly / Yearly Plan
  ↓
Start Purchase
  ↓
Google Play / App Store
  ↓
RevenueCat receives updated state
  ↓
CustomerInfo returned to client
  ↓
Aveli triggers billing sync
  ↓
Backend verifies subscription state
  ↓
Access is recalculated

A successful store transaction alone is not treated as the final workspace access decision.

The resulting entitlement must be reflected in the Aveli access model.

Restore Flow

The user may restore an existing subscription.

Restore Purchase
      ↓
RevenueCat
      ↓
Load CustomerInfo
      ↓
Billing Sync
      ↓
Backend Subscription State
      ↓
Access Resolution

If the restored subscription grants the support entitlement, workspace access can be restored.

Billing Synchronization

The client uses:

POST /v1/billing/sync

The purpose of billing sync is to reconcile external subscription state with the Aveli backend.

Conceptually:

RevenueCat State
      ↓
Aveli Backend
      ↓
Subscription Record
      ↓
Access Decision

The backend does not rely only on a client-side boolean such as:

isPremium = true

Instead, subscription state participates in the common access resolution model.

Webhook Flow

RevenueCat may also notify the backend directly:

RevenueCat
    ↓
POST /v1/webhooks/revenuecat
    ↓
Aveli Backend
    ↓
Update Subscription State

This allows subscription changes to be processed independently from the mobile application lifecycle.

Examples include:

renewal;
cancellation;
expiration;
product change;
entitlement change.
Entitlement Mapping

The store may contain multiple products:

Monthly Plan
Yearly Plan

Both map to:

support

The access layer therefore does not need to know which commercial plan was purchased to decide whether the subscription source is valid.

Store Product
     ↓
RevenueCat Entitlement
     ↓
support
     ↓
Subscription Access
Access Resolution

Subscription is one of several valid access sources:

Lifetime
   ↓
Manual
   ↓
Subscription
   ↓
Trial

RevenueCat only participates in the subscription branch.

It does not determine lifetime, manual or trial access.

Client Responsibility

The mobile application is responsible for:

initializing RevenueCat;
associating the current Aveli user with RevenueCat;
displaying available products;
starting purchase flow;
restoring purchases;
receiving CustomerInfo;
triggering backend billing synchronization;
refreshing application access state.
Backend Responsibility

The backend is responsible for:

receiving billing sync requests;
processing RevenueCat state;
storing normalized subscription information;
processing RevenueCat webhooks;
resolving subscription as part of the overall access decision.
Store Responsibility

Google Play and the App Store remain responsible for:

payment processing;
store product configuration;
recurring subscription lifecycle;
cancellation management;
localized subscription prices.

Aveli must not present hardcoded prices as authoritative store prices.

Security Boundary

The client contains only the public RevenueCat SDK key.

Mobile App
   ↓
Public SDK Key

Secret credentials remain server-side.

Backend
   ↓
RevenueCat Secret / Webhook Auth

Secret RevenueCat credentials must never be bundled into the mobile application.

Failure Scenarios
Purchase Succeeds but Sync Fails
Store Purchase
    ↓
RevenueCat updated
    ↓
Backend unavailable

The client should preserve the purchase result and allow access state to be reconciled later.

Local workspace data must not be affected.

Subscription Expires
Subscription expires
      ↓
RevenueCat state changes
      ↓
Backend subscription updated
      ↓
Access recalculated

If no other valid access source exists:

Access Gate

The workspace data remains stored locally.

Restore Finds No Valid Subscription
Restore
   ↓
No active support entitlement
   ↓
Backend access remains denied
   ↓
Access Gate

This is a valid business outcome rather than a technical failure.

Integration Boundary

RevenueCat is responsible for:

"What subscription does the store currently recognize?"

Aveli backend is responsible for:

"Does this account currently have workspace access?"

These questions are related, but they are not the same.

Summary

The Aveli billing flow follows this chain:

Store
  ↓
RevenueCat
  ↓
Aveli Backend
  ↓
Access Model
  ↓
Workspace

RevenueCat normalizes store subscription state, while Aveli keeps the final entitlement decision inside its own access model.