# Aveli — Trust Model

## Principle

Aveli использует **разные authorities для разных facts**.

Ни один component не trusted для всех system decisions.

## Authority Matrix

| Question | Authority |
|---|---|
| Who is authenticated account? | Aveli Backend |
| Is refresh session valid? | Aveli Backend |
| Does Aveli trial/grant exist? | Aveli Backend |
| What subscription state does RevenueCat recognize? | RevenueCat / store evidence |
| May workspace be opened online? | Aveli Backend |
| May verified state temporarily be trusted offline? | Frontend policy using backend snapshot |
| What clients/appointments/payments exist? | Local professional workspace |
| What visit-photo file exists locally? | Local workspace filesystem |
| What device contacts exist? | Device OS contact provider |
| Was local reminder scheduled? | Frontend + OS notification scheduler |

## Client Trust

Mobile client trusted чтобы:

```text
store local professional data
select correct per-user workspace
protect credentials/snapshot using secure storage
apply access verification policy
```

Он не trusted для invent server entitlement.

## Backend Trust

Backend trusted чтобы:

```text
validate identity/session
own trial/grants
normalize RevenueCat state
resolve access
```

Но не source of truth daily workspace records.

## RevenueCat Trust

RevenueCat trusted как provider subscription evidence.

Но не единственный Aveli access source: lifetime/manual/trial тоже существуют.

## Offline Trust

Offline access — bounded trust:

```text
previously verified server state
+
verification deadline
→ temporary client authorization
```

Это не permanent client-side entitlement.

## Failure Semantics

Trust failures должны fail в affected boundary:

```text
RevenueCat unavailable
→ billing verification fails

backend unavailable
→ access may rely on valid cached trust

contacts permission denied
→ contact import unavailable
```

Unrelated local workspace data остаются intact.
