# Dart http

> Thin HTTP transport used by Aveli auth/access data sources.

## Verified Version

`http 1.6.0`

## Role

The client uses explicit request construction for backend account/access APIs. There is no Dio dependency and no centralized HTTP interceptor middleware.

Bearer headers are added by data sources; access requests may perform a single 401 refresh/retry path.

## Replaceability

**High.** The wrapper is thin and isolated compared with state/domain layers.
