---
name: handwrite-io-track-order
description: >-
  Track a Handwrite card from submission through writing to mailing, and retrieve the proof
  images of the finished card and envelope. Use after a send, to confirm fulfillment, to
  reconcile an ambiguous send, or to attach proof of mailing to a CRM record.
api: handwrite-io:handwrite-io-orders-api
base_url: https://api.handwrite.io/v1
operations:
  - getOrder
generated: '2026-08-13'
method: generated
source: >-
  openapi/handwrite-io-orders-api-openapi.yml, https://documentation.handwrite.io/#get-an-order,
  conventions/handwrite-io-conventions.yml, lifecycle/handwrite-io-lifecycle.yml
consequence: read
human_in_the_loop: not-required
---

# Track a Handwrite order

## The constraint that shapes everything

Handwrite has **no webhooks, no callbacks and no event stream**, and **no list-orders endpoint**.
The only way to learn anything about an order is to fetch it by id, and the only way to get an id
is to have kept the one `POST /send` returned. If the id was not persisted, the order is
unreachable through the API — escalate to hello@handwrite.io.

## Fetch an order (`getOrder`)

```
GET https://api.handwrite.io/v1/order/{orderId}
Authorization: live_hw_...
```

`orderId` is the 24-character hex `_id` from the send response.

The response includes:

- `status` — the fulfillment state (below)
- `environment` — `live` or `test`, so you can confirm whether a card was really mailed
- `to` and `from` — the Recipient objects
- `message`, `handwriting`, `card`, `createdAt`
- `proofs` — once complete, an array of `{job_type, image_url}` where `job_type` is `card` or
  `envelope`

Note: the OpenAPI in this repository models `recipient`/`created_at`/`proof_url`, while
Handwrite's own payload uses `to`/`createdAt`/`proofs`. **Trust the provider's field names.**

## Status states

| `status` | Meaning | Terminal |
| --- | --- | --- |
| `processing` | Order accepted, not yet written | no |
| `written` | Written, not yet delivered | no |
| `complete` | Has been mailed; proofs available | yes |
| `problem` | Rare — technical issue on Handwrite's end, which they resolve | no |
| `cancelled` | Rare — Handwrite does not typically allow cancellations | yes |

`problem` is not a failure you can act on through the API. There is no retry or resubmit
operation. Surface it to a human and contact hello@handwrite.io.

## Polling loop

Fulfillment is a physical process measured in days, not seconds. Poll on that scale.

1. After sending, wait — do not poll in a tight loop. A reasonable cadence is hourly at most, and
   daily is usually enough.
2. Stop polling when `status` is `complete` or `cancelled`.
3. Budget the rate limit: **60 requests per minute per API key**, shared with every other call
   your integration makes. If you are tracking hundreds of orders, spread the fetches out; there
   is no bulk endpoint to help you.
4. There is no `Retry-After`. On a 429, sleep until the `X-RateLimit-Reset` epoch second.

## Retrieving proofs

When `status` is `complete`, `proofs` contains image URLs for the card and the envelope. Fetch
and store them if you need a record — treat the URLs as potentially expiring rather than
permanent, since Handwrite documents no retention policy for them.

## Reconciling an ambiguous send

This is the most important use of this skill. `POST /send` has no idempotency key, so after a
timeout or a 5xx you cannot tell whether cards were queued.

- If you captured order ids before the failure, fetch each one. An id that resolves means the
  order exists — do **not** resend it.
- If you captured no ids, do **not** resend. There is no way to search for orders by recipient.
  Escalate to a human, who can check the Handwrite web app.

## Errors

| Status | Meaning | What to do |
| --- | --- | --- |
| 401 | API key is wrong | Raw key in `Authorization`, no `Bearer` prefix |
| 404 | Order not found | Check the id; confirm the key is in the same environment (`live` vs `test`) as the order |
| 429 | Rate limited | Sleep until `X-RateLimit-Reset` |
| 503 | Maintenance | Retry later; there is no status page |
