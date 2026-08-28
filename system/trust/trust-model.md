# Aveli — Trust Model

## Principle

Aveli uses **different authorities for different facts**.

No single component is trusted for every system decision.

## Authority Matrix

| Question | Authority |
|---|---|
| Who is the authenticated account? | Aveli Backend |
| Is a refresh session valid? | Aveli Backend |
| Does an Aveli trial/grant exist? | Aveli Backend |
| What subscription state does RevenueCat recognize? | RevenueCat / store evidence |
| May the workspace be opened online? | Aveli Backend |
| May a verified state temporarily be trusted offline? | Frontend verification policy using backend snapshot |
| What clients/appointments/payments exist? | Local professional workspace |
| What visit-photo file exists locally? | Local workspace filesystem |
| What device contacts exist? | Device OS contact provider |
| Was a local reminder scheduled? | Frontend + OS notification scheduler |

## Client Trust

The mobile client is trusted to:

```text
store local professional data
select the correct per-user workspace
protect cached credentials/snapshot using secure storage
apply the access verification policy
```

It is not trusted to invent server entitlement.

## Backend Trust

The backend is trusted to:

```text
validate identity/session
own trial/grants
normalize RevenueCat state
resolve access
```

It is not the source of truth for daily workspace records.

## RevenueCat Trust

RevenueCat is trusted as provider subscription evidence.

It is not trusted as the only Aveli access source because lifetime/manual/trial also exist.

## Offline Trust

Offline access is bounded trust:

```text
previously verified server state
+
verification deadline
→ temporary client authorization
```

It is not permanent client-side entitlement.

## Failure Semantics

Trust failures should fail at the affected boundary:

```text
RevenueCat unavailable
→ billing verification fails

backend unavailable
→ access may rely on valid cached trust

contacts permission denied
→ contact import unavailable
```

Unrelated local workspace data remain intact.
