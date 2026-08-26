# Aveli — Domain Model

## Purpose

This document describes the main business entities in Aveli and the relationships between them.

The goal is to show the structure of the operational domain without tying it to a specific database schema.

---

## Core Domain

Aveli revolves around a small set of business entities:

```text
Client
  ↓
Appointment
  ↓
Visit
  ↓
Payment

Services, notes, photos and schedule rules support this core flow.

Client

A Client represents a person receiving services.

Typical attributes:

name;
phone;
contact details;
tags;
archive state;
visit history.

A client may have multiple appointments.

Client
  1
  │
  └── 0..* Appointments

Client data belongs to the local workspace.

Service

A Service represents a type of work provided by the specialist.

Typical attributes:

name;
price;
duration;
active state.

A service can be referenced by multiple appointments.

Service
  1
  │
  └── 0..* Appointments

Services are managed locally.

Appointment

Appointment is the central scheduling entity.

It connects:

Client
   +
Service
   +
Date / Time
   ↓
Appointment

Typical states include:

scheduled;
completed;
cancelled;
no-show.

An appointment may later produce visit-related data such as notes, photos and payment.

Visit

A Visit represents the completed working interaction associated with an appointment.

Conceptually:

Appointment
    ↓
Completed
    ↓
Visit Data

Visit-related data may include:

notes;
photos;
completion status;
payment state.

In the current implementation, visit information may be stored through appointment-related entities rather than as one standalone database object.

The domain distinction is still useful because scheduling and completed work have different business meaning.

Visit Note

A Visit Note stores additional information about a completed or active visit.

Relationship:

Appointment / Visit
        1
        │
        └── 0..* Visit Notes

Notes remain part of the local workspace.

Visit Photo

A Visit Photo represents media associated with a visit.

Metadata is related to the workspace entity, while the actual image file is stored in local device storage.

Appointment / Visit
        ↓
Photo Metadata
        ↓
Local File

Files are isolated by user.

Payment

Payment represents money received for completed work.

Typical information includes:

amount;
payment method;
payment status;
date;
associated visit.

Conceptually:

Completed Visit
      ↓
   Payment

A completed visit may also remain unpaid.

Completed Visit
      │
      ├── Paid
      │
      └── Outstanding

Outstanding payments are surfaced separately in the application.

Schedule

Schedule represents the specialist's working availability.

It provides rules used when creating or moving appointments.

Working Hours
     +
Existing Appointments
     ↓
Slot Availability

Scheduling logic prevents invalid or conflicting appointment states according to the current business rules.

Account

Account belongs to a different domain from the local workspace.

It represents the user's server-side identity.

Account
   ↓
Session
   ↓
Access

Account does not own clients, appointments or payments in the current architecture.

Session

Session represents an authenticated interaction with the backend.

It is responsible for:

authenticated API access;
access token lifecycle;
refresh token lifecycle.

Session state does not contain workspace data.

Access

Access represents whether the current account is allowed to open the workspace.

Possible sources include:

Lifetime
Manual Grant
Subscription
Trial
None

Access is evaluated independently from operational workspace ownership.

Subscription

Subscription represents store-backed access support.

Conceptually:

Store
  ↓
RevenueCat
  ↓
Subscription State
  ↓
Aveli Backend
  ↓
Access Decision

Monthly and yearly products map to the same logical entitlement.

High-Level Relationships
Account
  │
  ├── Session
  ├── Access
  └── Subscription

Local Workspace
  │
  ├── Client
  │     └── Appointment
  │            ├── Service
  │            ├── Visit Note
  │            ├── Visit Photo
  │            └── Payment
  │
  └── Schedule

The key point is that the Account tree and Workspace tree are connected by access control, not by shared data ownership.

Domain Boundary
SERVER DOMAIN

Account
Session
Access
Subscription

        │
        │ controls availability
        ▼

LOCAL DOMAIN

Client
Service
Appointment
Visit
Payment
Schedule
Notes
Photos

The server decides whether the local domain can be opened.

It does not own the local domain entities themselves.

Summary

The Aveli domain can be understood as two connected models:

Identity & Entitlement
        +
Operational Workspace

The first determines access.

The second contains the user's actual professional activity.