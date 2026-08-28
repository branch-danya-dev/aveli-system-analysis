# Aveli — Main User Journey

<p align="center">
  <a href="main-user-journey.md"><b>English</b></a> ·
  <a href="main-user-journey.ru.md">Русский</a>
</p>

> Describes the primary user journey from first product entry to everyday professional workspace usage.

---

## Purpose

The main journey connects the major Aveli product stages:

```text
Identity
   ↓
Access
   ↓
Workspace
   ↓
Daily Work
```

It provides a business-level view of how a specialist enters the product and continues normal professional activity.

Technical startup, authentication, storage, and access-resolution behavior are documented separately.

---

## New User Journey

A new user follows the primary entry path:

```text
Install Aveli
    ↓
Launch application
    ↓
Welcome / Authentication
    ↓
Create account
    ↓
Trial becomes active
    ↓
Access is available
    ↓
Open Workspace
```

A new account receives the current Aveli trial defined by the product rules.

During a valid trial, the user receives access to the complete workspace.

Related rules:

`BR-001`–`BR-013`

---

## Returning User Journey

For an existing user:

```text
Launch Aveli
    ↓
Existing authenticated state available?
   /                         \
 Yes                         No
  ↓                           ↓
Determine access          Sign in
  ↓                           ↓
Access available?        Determine access
   /        \                  ↓
 Yes        No             Continue below
  ↓          ↓
Workspace  Access Gate
```

The workspace is opened only after the product has determined that the current user has valid access.

If the user is not authenticated, they must enter through the authentication flow first.

---

## Workspace Entry

After access is granted, the user enters the professional workspace.

The main product areas are:

```text
Today
Calendar
Clients
More
```

These areas organize the specialist's normal daily activity.

---

## Today Journey

The Today area provides a working view of the current day.

Typical use:

```text
Open Today
    ↓
Review today's activity
    ↓
Open an appointment
    ↓
Perform or update work
    ↓
Return to daily overview
```

The user may also create a new appointment from the daily workflow.

Today should reflect relevant appointment lifecycle changes such as rescheduling, cancellation, no-show, and completion.

---

## Calendar Journey

The Calendar supports planning across dates.

Typical flow:

```text
Open Calendar
    ↓
Select date
    ↓
Review scheduled work
    ↓
Open or create appointment
    ↓
Update schedule if required
```

The Calendar is another view of the specialist's appointment workflow rather than an independent source of professional activity.

---

## Appointment Journey

Appointments are one of the main working entities in Aveli.

Typical creation flow:

```text
Choose date or start new appointment
    ↓
Select or create client
    ↓
Select service
    ↓
Choose date and time
    ↓
Confirm appointment
    ↓
Appointment appears in the schedule
```

During the appointment lifecycle, the user may:

- reschedule;
- cancel;
- mark no-show;
- complete the visit.

A completed visit may also preserve:

- notes;
- visit photos;
- payment information.

Detailed appointment rules are defined in:

[`../requirements/business-rules.md`](../requirements/business-rules.md)

---

## Client Journey

Typical client flow:

```text
Open Clients
    ↓
Search or create client
    ↓
Open client profile
    ↓
Review client information
    ↓
Review available visit history
    ↓
Continue professional work
```

The user may also create an Aveli client from a device contact when the required permission is available.

A client profile acts as a point of access to the professional context associated with that client.

---

## Payment Journey

Payment is associated with professional work.

Typical flow:

```text
Complete visit
    ↓
Payment received?
   /              \
 Yes              No
  ↓                ↓
Record payment   Keep as outstanding
                    ↓
               Resolve later
```

An unpaid completed visit remains available to the user until the payment state is resolved.

Aveli provides simple payment tracking and does not turn this flow into a full accounting process.

---

## Access Expiration Journey

When the user's current access is no longer valid:

```text
Access expires
    ↓
Access is evaluated
    ↓
Another valid access source exists?
   /                           \
 Yes                           No
  ↓                             ↓
Workspace remains available   Access Gate
```

Existing professional workspace information is preserved.

After the user restores or obtains a valid access source:

```text
Valid access restored
    ↓
Workspace becomes available
    ↓
Existing professional information remains available
```

Loss of access changes workspace availability, not ownership of professional information.

---

## Offline Journey

Aveli supports offline-oriented daily work.

When connectivity is unavailable:

```text
User opens or continues workspace
    ↓
Previously verified access still sufficient?
   /                                \
 Yes                                No
  ↓                                  ↓
Continue supported work        Verification required
```

Supported professional workspace activity should remain available while the current offline-access policy allows it.

Operations requiring current account, access, or subscription verification wait for connectivity.

The exact technical mechanism is documented in:

```text
../../frontend/offline/
../../backend/access/
```

---

## Logout and Account Switching

When the user logs out:

```text
Active user
    ↓
Logout
    ↓
Authenticated state ends
    ↓
Current workspace closes
```

Persistent professional information remains preserved.

If another user signs in:

```text
New authenticated user
    ↓
Determine access
    ↓
Open that user's workspace
```

Information belonging to the previous user must not appear in the newly active workspace.

---

## Journey Summary

The primary Aveli product journey is:

```text
Enter product
    ↓
Establish identity
    ↓
Determine access
    ↓
Open personal workspace
    ↓
Plan work
    ↓
Work with clients
    ↓
Complete visits
    ↓
Record professional context
    ↓
Continue daily workflow
```

Authentication and commercial access determine whether the workspace may be opened.

The professional workspace represents the user's ongoing day-to-day work and remains conceptually separate from the access lifecycle.

---

## Related Documentation

- [`../context/product-context.md`](../context/product-context.md)
- [`../scope/scope.md`](../scope/scope.md)
- [`access-journey.md`](access-journey.md)
- [`../requirements/business-rules.md`](../requirements/business-rules.md)
- [`../requirements/functional-requirements.md`](../requirements/functional-requirements.md)
- [`../../frontend/`](../../frontend/)
- [`../../backend/`](../../backend/)
