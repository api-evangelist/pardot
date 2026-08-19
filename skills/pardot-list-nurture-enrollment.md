---
name: pardot-list-nurture-enrollment
description: Add Marketing Cloud Account Engagement (Pardot) prospects to a list so an Engagement Studio nurture program can pick them up, and audit membership afterwards.
api: pardot:account-engagement-api-v5
base_url: https://pi.pardot.com/api/v5
generated: '2026-08-13'
method: generated
source:
  - https://developer.salesforce.com/docs/marketing/pardot/guide/list-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/list-membership-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/engagement-studio-program-v5.html
operations:
  - GET /api/v5/objects/lists
  - POST /api/v5/objects/lists
  - GET /api/v5/objects/list-memberships
  - POST /api/v5/objects/list-memberships
  - PATCH /api/v5/objects/list-memberships/{id}
  - DELETE /api/v5/objects/list-memberships/{id}
  - GET /api/v5/objects/engagement-studio-programs
---

# Enroll prospects in a nurture list

Account Engagement does not have an "enroll in program" endpoint. Nurture entry is
list-driven: you put a prospect on a list, and the Engagement Studio program that is
built on that list picks them up. This skill covers the list side, which is the part
the API actually exposes.

## Step 1 — Find or create the list

`GET /api/v5/objects/lists?fields=id,name,isPublic,isDynamic&orderBy=id ASC`

`fields` is required on every query — there is no default representation.

If the list does not exist, `POST /api/v5/objects/lists` with a `name`. Be aware of
the distinction: a **dynamic** list is maintained by Account Engagement rules and you
should not write memberships into it by hand; a **static** list is the one to drive
from an integration.

## Step 2 — Add the membership

`POST /api/v5/objects/list-memberships?fields=id,listId,prospectId,optedOut`

```json
{ "listId": 1234, "prospectId": 565722 }
```

There is no idempotency key. Before writing, query for an existing membership:

`GET /api/v5/objects/list-memberships?fields=id,prospectId&listId=1234&prospectId=565722`

Re-posting a membership that already exists is not a safe no-op — check first.

## Step 3 — Suppress rather than delete when the intent is "stop mailing"

`PATCH /api/v5/objects/list-memberships/{id}` to flip the opt-out state keeps the
audit trail. `DELETE /api/v5/objects/list-memberships/{id}` removes the row entirely
and the prospect silently drops out of anything keyed on that list.

## Step 4 — Audit at scale with cursor pagination

`GET /api/v5/objects/list-memberships?fields=id,prospectId,listId,createdAt&listId=1234`

Then follow `nextPageToken` / `nextPageUrl` from the response envelope:

```json
{ "values": [ ... ], "nextPageToken": "...", "nextPageUrl": "https://pi.pardot.com/..." }
```

Rules that will bite you:

- When you send `nextPageToken`, **only** `fields` may accompany it. Sending `orderBy`,
  `offset`, `limit` or any filter alongside a token returns `400 BAD_REQUEST`.
- A token expires after **4 hours**.
- A token sequence stops generating after **100,000 records** and the response carries
  `Pardot-Warning: 203`. Past that, use the Export API rather than paging.
- `limit` maxes at 1000 and defaults to 200. `offset` is deprecated and capped at 2000.

## Step 5 — Confirm the program is listening

`GET /api/v5/objects/engagement-studio-programs?fields=id,name,status,listIds`

The program surface is read-oriented; you cannot author or start a program over the API.
Confirm the program is in the expected status and that your list is one it draws from.

## Budget and failure

Cap concurrency at **five** requests (code 66 above that) and stay inside the daily
allocation for the edition (code 122). A large membership audit is exactly the workload
that exhausts both — page serially, and send `X-Return-Api-Usage: 1` so the
`x-api-usage` header tells you how much of the day's budget you have spent.

Full code registry: `errors/pardot-error-codes.yml`. Shared semantics:
`conventions/pardot-conventions.yml`.
