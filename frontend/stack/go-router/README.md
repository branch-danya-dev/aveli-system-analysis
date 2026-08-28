# go_router

> Navigation and route-guard technology of Aveli client.

## Verified Version

`go_router 14.8.1`

## Role

The router owns `/bootstrap` startup navigation, a `StatefulShellRoute` with four workspace tabs, global auth/access redirects, query-based date deep links, and root-navigator appointment details.

## Why It Fits Current Structure

Auth and access policy are centralized in one redirect callback rather than repeated on every screen.

## Replaceability

**Medium.** Navigation behavior is centralized, but routes, shell semantics, deep links and redirect tests would need coordinated migration.
