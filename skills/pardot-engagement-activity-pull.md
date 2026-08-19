---
name: pardot-engagement-activity-pull
description: Pull Marketing Cloud Account Engagement (Pardot) visitor, visit and activity data out to an external analytics store without blowing the daily API allocation.
api: pardot:account-engagement-api-v5
base_url: https://pi.pardot.com/api/v5
generated: '2026-08-13'
method: generated
source:
  - https://developer.salesforce.com/docs/marketing/pardot/guide/visitor-activity-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/visit-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/export-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html
operations:
  - GET /api/v5/objects/visitor-activities
  - GET /api/v5/objects/visitor-activities/{id}
  - GET /api/v5/objects/visits
  - GET /api/v5/objects/visitors
  - GET /api/v5/objects/visitor-page-views
  - POST /api/v5/exports
  - GET /api/v5/exports/{id}
---

# Pull engagement activity into an external store

Two ways out of Account Engagement, and picking the wrong one is what exhausts the
daily allocation.

## Decide first: object API or Export API

| Situation | Use |
|---|---|
| Incremental sync, thousands of rows, near-real-time | Object query + `nextPageToken` |
| Backfill, tens of thousands to millions of rows | **Export API** |
| More than 100,000 records in one logical pull | **Export API** — cursor paging stops there |
| More than 2,000 records with `offset` | **Export API** — the docs say so explicitly |

The Export API is asynchronous: you create an export job, poll it, then download the
result files. It costs a handful of API calls for an arbitrarily large dataset, where
paging the object API costs one call per page.

## Incremental pull with the object API

```
GET /api/v5/objects/visitor-activities
    ?fields=id,type,typeName,createdAt,prospect.id,prospect.email,visitor.id
    &orderBy=id ASC
    &limit=1000
Authorization: Bearer <access_token>
Pardot-Business-Unit-Id: <business_unit_id>
X-Return-Api-Usage: 1
```

- `fields` is **required**. Dot notation pulls related objects in the same call, up to
  three object tracks deep — `prospect.email` costs nothing extra. Use this instead of
  a second lookup per row; it is the single biggest call-count saving available.
- Sort by `id ASC` and keep a high-water mark so a restart resumes rather than replays.
- Follow `nextPageToken` / `nextPageUrl`. Send **only** `fields` alongside a token —
  anything else returns `400`. Tokens die after 4 hours; a sequence stops at 100,000
  records with `Pardot-Warning: 203`.
- `deleted=all` if you need recycle-bin rows for reconciliation.

## Read a single record cheaply

`GET /api/v5/objects/visitor-activities/{id}` accepts `If-Modified-Since` (RFC 7231)
and returns `Last-Modified`. An unchanged record answers `304 Not Modified` with an
empty body. For re-verification loops this is materially cheaper than refetching.

## Stay inside the budget

- **Concurrency is hard-capped at five.** A sixth in-flight request fails with code 66.
  This is not a soft limit and there is no `Retry-After` to tell you when to come back.
- **Daily allocation is by edition:** Growth 25,000, Plus 50,000, Advanced and Premium
  100,000 requests per day, resetting at start-of-day in the account time zone.
  Exhaustion is code **122**.
- There are no `RateLimit-*` headers. The only usage signal is the `x-api-usage`
  response header, and it only appears if you send `X-Return-Api-Usage: 1`. Read it on
  every page and stop the job yourself before you hit 122.

## Freshness caveat

v5 reads may be served from a cache up to 60 seconds behind the primary dataset. Do not
build an alerting path that assumes sub-minute freshness, and do not treat a missing
just-created row as data loss.

## Errors

`{"code": <int>, "message": "..."}` — see `errors/pardot-error-codes.yml` for all 208
published codes. Shared runtime semantics: `conventions/pardot-conventions.yml`.
Published ceilings: `rate-limits/pardot-rate-limits.yml`.
