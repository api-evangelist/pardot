---
name: pardot-bulk-action-job
description: Run and supervise a Marketing Cloud Account Engagement (Pardot) bulk action job — mass delete or mass update of up to 100,000 records — including pause, resume, cancel and error-report download.
api: pardot:account-engagement-api-v5
base_url: https://pi.pardot.com/api/v5
generated: '2026-08-13'
method: generated
source:
  - https://developer.salesforce.com/docs/marketing/pardot/guide/bulk-actions-v5.html
  - https://developer.salesforce.com/docs/marketing/pardot/guide/import-v5.html
operations:
  - POST /api/v5/bulk-actions
  - GET /api/v5/bulk-actions
  - GET /api/v5/bulk-actions/{id}
  - PATCH /api/v5/bulk-actions/{id}
  - GET /api/v5/bulk-actions/{id}/errors
---

# Run a bulk action job

Bulk actions are the v5-only path for mass operations — up to **100,000 prospects in a
single request**. They are asynchronous and destructive by default, so this is the
flow that most needs a human in the loop before it runs.

> **Escalate before executing.** A bulk delete against the wrong CSV removes up to
> 100,000 prospect records. An agent should surface the object, the action, the row
> count and a sample of the selected records for confirmation before creating the job.

## Step 1 — Create the job

```
POST /api/v5/bulk-actions?fields=id,status,createdBy.id
Authorization: Bearer <access_token>
Pardot-Business-Unit-Id: <business_unit_id>
Content-Type: multipart/form-data
```

The request names the `object`, the `action`, the `status` to start in, the selected
fields, and carries the CSV of target records as the file part. Because a file is
attached, this is a `multipart/form-data` request with an `input` part (the JSON input
representation) and a `file` part — the same convention every file-bearing v5 create
uses.

A successful create returns `201` with a `Location` header, e.g.
`https://pi.pardot.com/api/v5/bulk-actions/101?fields=id,status,createdBy.id`.

**There is no idempotency key.** If the create times out, do not blindly retry — query
`GET /api/v5/bulk-actions` first and check whether the job already landed.

## Step 2 — Poll the job

`GET /api/v5/bulk-actions/{id}?fields=id,status,object,action,createdAt,updatedAt`

Poll with backoff. Every poll spends one call from the daily allocation, and the
allocation is 25,000/day on Growth — a tight polling loop on a long job is a real way
to lose the rest of the day's budget. Send `X-Return-Api-Usage: 1` and watch
`x-api-usage`.

## Step 3 — Control it mid-flight

All three controls are the same `PATCH /api/v5/bulk-actions/{id}` with a status:

| Intent | Body |
|---|---|
| Pause | `{"status": "Paused"}` |
| Resume | `{"status": "Ready"}` |
| Cancel | `{"status": "Cancelled"}` |

Pause is the safety valve. If a job is doing something unexpected, pause first and
inspect — do not wait for it to finish.

## Step 4 — Read the error report

`GET /api/v5/bulk-actions/{id}/errors`

Downloads the report of records the completed job failed to process. A job that reports
"complete" can still have failed rows; always pull this before declaring success.

## Step 5 — Reconcile

Remember the read-cache lag: v5 reads can trail the primary dataset by up to 60 seconds.
Immediately after a bulk delete, a query may still return records that are already gone.
Wait, then reconcile, and query with `deleted=all` to distinguish "moved to the recycle
bin" from "permanently gone" — v5 delete semantics differ by object type, and deletes
never cascade to related objects.

## Failure handling

- **code 122** — daily allocation exhausted. The job itself is not the cost; the polling
  usually is.
- **code 66** — more than five concurrent requests. Bulk jobs plus a parallel sync will
  trip this.
- **405 + code 108** — a target record is referenced elsewhere and cannot be deleted.

Full registry: `errors/pardot-error-codes.yml`. Agent execution contracts for the
underlying operations: `agentic-access/pardot-agentic-access.yml`.
