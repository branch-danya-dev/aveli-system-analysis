# Aveli — Main User Journey

## Purpose

This document describes the primary user journey from application launch to everyday workspace usage.

---

## Entry Flow

A new user follows this path:

```text
Install Aveli
    ↓
Launch application
    ↓
Bootstrap
    ↓
Welcome / Auth
    ↓
Register account
    ↓
Server creates 30-day trial
    ↓
Access granted
    ↓
Open Workspace

The user receives full workspace access during the active trial period.

Returning User

For an existing user:

Launch application
    ↓
Restore session
    ↓
Open local user database
    ↓
Verify access state
    ↓
Access valid?
   /          \
 Yes          No
  ↓            ↓
Workspace    Access Gate

The access check happens before the main workspace is shown.

Main Workspace Journey

After access is granted, the user enters the main workspace.

Today
  │
  ├── View daily schedule
  ├── Create appointment
  └── Review daily metrics

Calendar
  │
  ├── View week / day
  ├── Create appointment
  └── Reschedule appointment

Clients
  │
  ├── Find client
  ├── Create client
  ├── Open history
  └── Manage client data

More
  │
  ├── Services
  ├── Unpaid visits
  ├── Profile
  ├── Schedule settings
  ├── Reminders
  └── Appearance
Appointment Journey

Appointments are one of the main working entities.

Typical flow:

Select date
    ↓
Create appointment
    ↓
Select / create client
    ↓
Select service
    ↓
Choose date and time
    ↓
Save appointment
    ↓
Appointment appears in schedule

During the visit, the user may:

add notes;
attach visit photos;
mark the visit as completed;
accept payment;
reschedule;
cancel;
mark the client as no-show.
Client Journey
Clients
   ↓
Search or create client
   ↓
Open client profile
   ↓
View contact details
   ↓
View appointment history
   ↓
Create next appointment

Clients may also be imported from device contacts.

Payment Journey

Payments are associated with completed work.

Appointment / Visit
        ↓
Complete visit
        ↓
Payment received?
      /          \
    Yes          No
     ↓            ↓
Record payment   Outstanding payment

Unpaid items remain available in the dedicated outstanding-payments view.

Trial Expiration

When the user's access period ends:

Trial expires
      ↓
Access verification
      ↓
Access denied
      ↓
Access Gate
      ↓
Support / Subscription screen

The user's local workspace data is not deleted.

After subscription purchase, restore or another valid access grant:

Access restored
      ↓
Workspace available again
      ↓
Existing local data remains available
Offline Journey

While a valid cached access state exists, the user can continue working with local data during the allowed offline grace period.

Available offline:

clients;
appointments;
services;
payments;
notes;
photos;
local reminders.

Operations requiring the backend wait until connectivity is restored.

Summary

The main Aveli journey follows a simple pattern:

Identity
   ↓
Access
   ↓
Workspace
   ↓
Daily Work

Account and billing determine whether the workspace can be opened, while everyday professional data remains local to the device.