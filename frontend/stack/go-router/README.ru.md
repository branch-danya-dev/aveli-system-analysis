# go_router

> Navigation и route-guard technology Aveli client.

## Verified Version

`go_router 14.8.1`

## Role

Router владеет `/bootstrap` startup navigation, `StatefulShellRoute` с четырьмя workspace tabs, global auth/access redirects, date query deep links и root-navigator appointment details.

## Почему подходит текущей структуре

Auth/access policy централизована в одном redirect callback, а не дублируется по screens.

## Replaceability

**Medium.** Navigation centralized, но routes, shell semantics, deep links и redirect tests потребуют coordinated migration.
