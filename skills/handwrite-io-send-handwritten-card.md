---
name: handwrite-io-send-handwritten-card
description: >-
  Write and mail a real handwritten card through Handwrite. Use when a workflow needs physical
  handwritten correspondence sent to one or more US postal addresses — a thank-you after a
  purchase, a follow-up after a meeting, a donor acknowledgement. This skill mails physical mail
  and spends money; it must not run without human confirmation.
api: handwrite-io:handwrite-io-send-api
base_url: https://api.handwrite.io/v1
operations:
  - getHandwritings
  - getStationery
  - sendLetter
  - getOrder
generated: '2026-08-13'
method: generated
source: >-
  openapi/handwrite-io-*-openapi.yml, https://documentation.handwrite.io/,
  conventions/handwrite-io-conventions.yml, errors/handwrite-io-problem-types.yml
consequence: physical
human_in_the_loop: required
---

# Send a handwritten card

## Before you start

Handwrite's `POST /send` is not a draft, a preview, or a queue. Each recipient in a live-mode
request becomes a card that is physically written, stamped and mailed, and billed at $2.99 down
to $2.45 depending on volume. Handwrite states cancellations are not typically allowed. There is
**no idempotency key**, so a retry can mail the same card twice.

Confirm with a human before every live send. Rehearse with a `test_hw` key first.

## Authentication

Send the raw key as the entire `Authorization` header value — no `Bearer` prefix.

```
Authorization: live_hw_...
Content-Type: application/json
```

Keys are created at `https://app.handwrite.io/integrations/api`. A key beginning `test_hw` is not
billed and mails nothing; `live_hw` is billed and mails. The mode is decided by the key, not by
the URL. Never call this API from a browser — the provider forbids it, and there is no
publishable key.

## Step 1 — choose a handwriting style (`getHandwritings`)

```
GET https://api.handwrite.io/v1/handwriting
```

Returns a bare JSON array (no envelope, no paging) of `{_id, name, preview_url}`. Keep the `_id`.

## Step 2 — choose stationery (`getStationery`)

```
GET https://api.handwrite.io/v1/stationery
```

Same shape. Returns Handwrite's public card options plus any custom stationery the account
uploaded. The card you pick also determines whether the message prints on the front or the back.
Keep the `_id`.

**Do not confuse the two ids.** Both are bare 24-character hex ObjectIds with no type prefix. If
you pass a handwriting id where `card` is expected, nothing will stop you — the card still mails,
wrong.

## Step 3 — build the request

```json
{
  "message": "Thanks for stopping by last week — it was a pleasure. \n\nBest,\n-Alex",
  "handwriting": "<Handwriting _id from step 1>",
  "card": "<Stationery _id from step 2>",
  "recipients": [
    {
      "firstName": "Jamie",
      "lastName": "Stockton",
      "company": "Stockton Lumber",
      "street1": "8284 Random Road",
      "city": "Sarasota",
      "state": "FL",
      "zip": "34240"
    }
  ],
  "from": {
    "firstName": "Alex",
    "lastName": "Reed",
    "street1": "293 Hungerford Drive",
    "city": "Rockville",
    "state": "MD",
    "zip": "20850"
  }
}
```

Hard constraints, all enforced by Handwrite:

- `message` — required, maximum **320 characters**. Count before sending; there is no truncation.
- `handwriting`, `card` — required ids from steps 1 and 2.
- `recipients` — required, **1 to 10** objects. Each needs `firstName`, `lastName`, `street1`,
  `city`, `state`, `zip`.
- `state` — capitalized **two-letter US abbreviation** (`AL`, not `Alabama`).
- `zip` — exactly **5 characters**.
- `company` — optional; when present it prints on the first address line with attention-to on the
  second.
- `from` — optional return address, same Recipient shape.

There is no country field. This API mails inside the US only.

## Step 4 — send (`sendLetter`)

```
POST https://api.handwrite.io/v1/send
```

Returns an **array** of Order objects — one per recipient. Each carries `_id`, `status`
(`processing` on creation), `environment` (`live` or `test`), `to`, `from`, `message` and
`createdAt`.

**Persist every `_id` immediately.** Handwrite has no list-orders endpoint. An order id you do
not store is an order you can never look up again.

### Batch mode

The same endpoint accepts an **array** of the object above. The cap is **1,000 orders per
request**, where one order is one message to one recipient. Handwrite does not document what
happens when a single element of a batch is invalid, so do not assume the request is
all-or-nothing — check the returned array length against what you sent.

## Step 5 — confirm, never blind-retry

If the call times out or returns 5xx, **do not resend.** You cannot tell whether the cards were
already queued. Instead, if you captured any order ids, poll `getOrder`; if you captured none,
escalate to a human rather than risk duplicate mail.

See the companion skill `handwrite-io-track-order` for the polling loop.

## Errors

| Status | Meaning | What to do |
| --- | --- | --- |
| 400 | Your request is invalid | Re-check the 320-char message limit, the 1-10 recipient count, the 2-letter state and the 5-char zip |
| 401 | Your API key is wrong | Send the raw key with no `Bearer` prefix and set `Content-Type: application/json` |
| 429 | Too many requests (`rate_limit_exceeded`) | Limit is 60/min per key. There is no `Retry-After`; sleep until the `X-RateLimit-Reset` epoch second |
| 500 | Server error | Do **not** blind-retry a send — reconcile first |
| 503 | Maintenance | Retry later; there is no status page to check |

Error bodies are plain JSON (`{"message": "..."}`), not RFC 9457 problem+json.

## Rate limiting

60 requests per minute per API key, flat for every account. Read `X-RateLimit-Remaining` and back
off before you hit zero. A 1,000-order batch is a single request — batching is the correct way to
stay under the limit, not a loop of single sends.

## Cost

Live sends bill per mailed card: $2.99 (1-99), $2.75 (100-249), $2.60 (250-499), $2.45 (500-999),
negotiated above 999. The price includes card, envelope, writing and postage. A 10-recipient
request is 10 cards, not one.
