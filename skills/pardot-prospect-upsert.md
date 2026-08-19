---
name: pardot-prospect-upsert
description: Create or update a Marketing Cloud Account Engagement (Pardot) prospect from an external system without producing duplicates, and attach it to a campaign.
api: pardot:account-engagement-api-v5
base_url: https://pi.pardot.com/api/v5
generated: '2026-08-13'
method: generated
source:
  - https://developer.salesforce.com/docs/marketing/pardot/guide/prospect-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/version5overview.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/authentication.html
operations:
  - POST /api/v5/objects/prospects/do/upsertLatestByEmail
  - POST /api/v5/objects/prospects
  - GET /api/v5/objects/prospects
  - PATCH /api/v5/objects/prospects/{id}
  - POST /api/v5/objects/prospects/{id}/do/addTag
---

# Upsert a prospect into Account Engagement

Use this when an external system (a CRM, a webinar platform, a product signup) has a
person and you need Account Engagement to hold exactly one prospect for them.

## Before you call anything

1. Get a Salesforce OAuth 2.0 access token. The connected app **must** have the
   `pardot_api` scope, and the user must be SSO-enabled.
2. Get the Account Engagement Business Unit ID — 18 characters, begins `0Uv`, from
   Salesforce Setup > Business Unit Setup.
3. Pick the right host. Production is `pi.pardot.com` (Salesforce domain
   `login.salesforce.com`). Developer orgs and sandboxes are `pi.demo.pardot.com`
   (sandboxes authenticate against `test.salesforce.com`).

Every request carries both headers:

```
Authorization: Bearer <access_token>
Pardot-Business-Unit-Id: <business_unit_id>
```

## Step 1 — Prefer the upsert, not a create

`POST /api/v5/objects/prospects/do/upsertLatestByEmail`

This finds the most recently updated prospect with the supplied `email` and updates it;
if none exists it creates one. **This is the only idempotent write in the API.** There is
no idempotency-key header, so a retried `POST /api/v5/objects/prospects` will create a
second prospect — and if the business unit has AMPSEA (Allow Multiple Prospects with the
Same Email Address) enabled, it will do so every single time.

Send `fields` on the query string to control what comes back:

```
POST /api/v5/objects/prospects/do/upsertLatestByEmail?fields=id,email,firstName,lastName
Content-Type: application/json

{ "matchEmail": "jdoe@company.com",
  "prospect": { "firstName": "John", "lastName": "Doe", "campaignId": 668 } }
```

Check the object reference for the exact input representation before sending — the
request shape for the `do/` actions is documented on the Prospect page, not inferable
from the collection endpoints.

## Step 2 — If you must create explicitly

`POST /api/v5/objects/prospects` with `email` (the only required editable field).

- `201 Created` returns the requested `fields` and a `Location` header pointing at the
  read URL.
- `204 No Content` means the record WAS created but the API user cannot read it back.
  Do not treat 204 as a failure and retry — that is how duplicates get made.

## Step 3 — Update by id, never by email

`PATCH /api/v5/objects/prospects/{id}`. In v5 an update must name the prospect by
Account Engagement numeric id; the v3 behaviour of updating by email address is gone.
Omitted fields are left unchanged.

## Step 4 — Verify, but expect a lag

v5 reads are served from a cache that can trail the primary dataset by up to 60 seconds.
An immediate read-after-write can return the pre-write state. If you must confirm, poll
with backoff rather than failing on the first stale read.

## Step 5 — Tag it if you need segmentation

`POST /api/v5/objects/prospects/{id}/do/addTag` — requires both
*Prospect > Prospects > Create* and *Marketing > Segmentation > Tags > Create* abilities.

## Failure handling

Errors come back as `{"code": <int>, "message": "..."}` — a numeric code registry, not
RFC 9457 problem details. The HTTP status is coarse; the body code carries the meaning.
See `errors/pardot-error-codes.yml`.

- **code 122** — daily API allocation exhausted (25,000–100,000 calls/day depending on
  edition). Stop for the day; the counter resets at the start of day in the account time
  zone. Do not hot-retry.
- **code 66** — more than five concurrent requests. Cap your own concurrency at five.
  This is the single most common self-inflicted failure on this API.
- **405 + code 108** — the record is referenced elsewhere and cannot be deleted.
- **Pardot-Warning: 201;Record In Recycle Bin** — the target is soft-deleted. Query with
  `deleted=all` to see it.

To watch your budget, send `X-Return-Api-Usage: 1` and read the `x-api-usage` response
header. It is opt-in — you get no usage signal at all unless you ask for it, and there
are no `RateLimit-*` headers and no `Retry-After`.

## Permissions

Every operation is gated by an Account Engagement role ability, listed per operation on
the object reference page. Create/update need *Prospect > Prospects > Create*; reading
prospects not assigned to the API user needs
*Prospects > Prospects > View prospects not assigned to self*.
