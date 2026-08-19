---
name: handwrite-io-browse-options
description: >-
  List the handwriting styles and stationery available on a Handwrite account, with preview
  images, so a human or an agent can choose before sending. Use to build a picker, to cache the
  ids a send workflow needs, or to check whether custom stationery has been uploaded. Read-only
  and free — nothing is mailed and nothing is billed.
api: handwrite-io:handwrite-io-handwriting-api
base_url: https://api.handwrite.io/v1
operations:
  - getHandwritings
  - getStationery
generated: '2026-08-13'
method: generated
source: >-
  openapi/handwrite-io-handwriting-api-openapi.yml,
  openapi/handwrite-io-stationery-api-openapi.yml,
  https://documentation.handwrite.io/#get-handwritings,
  https://documentation.handwrite.io/#get-stationery
consequence: read
human_in_the_loop: not-required
---

# Browse handwriting styles and stationery

These are the two lookup calls every send depends on. `POST /send` takes a `handwriting` id and a
`card` id and will not accept names, so this is where those ids come from.

## Authentication

```
Authorization: test_hw_...
Content-Type: application/json
```

Both operations are read-only and are safe to run with a **test** key — nothing is mailed and
nothing is billed. Use a test key when building a picker.

## List handwriting styles (`getHandwritings`)

```
GET https://api.handwrite.io/v1/handwriting
```

Returns a bare JSON array — no envelope, no `data` wrapper, no pagination, no total:

```json
[
  { "_id": "...", "name": "Jeremy",  "preview_url": "https://res.cloudinary.com/handwrite/..." },
  { "_id": "...", "name": "Tribeca", "preview_url": "https://res.cloudinary.com/handwrite/..." }
]
```

`preview_url` is a rendered sample of the style, suitable for showing a human in a chooser.

## List stationery (`getStationery`)

```
GET https://api.handwrite.io/v1/stationery
```

Same shape. Returns **both** the public card options Handwrite provides **and** any custom
stationery the account has uploaded, so the result is account-specific — do not cache it across
accounts. The card chosen also determines whether the message prints on the front or the back of
the card.

## Working with the ids

- Every `_id` is a bare 24-character hexadecimal ObjectId with **no type prefix**. A handwriting
  id and a card id are indistinguishable by inspection.
- Because `POST /send` takes both as plain strings, transposing them is a silent error that still
  mails a card. Keep them in named fields, never in one list.
- There is no get-by-id operation for either resource. If you need to resolve an id back to a
  name, cache the list.

## Caching and rate limits

Both lists are small and change rarely — Handwrite's own catalog is stable, and custom stationery
changes only when someone uploads it. Cache the results rather than calling before every send.

The limit is 60 requests per minute per API key, shared across all four operations. In a bulk
send workflow, fetch these two lists **once** at the start; do not fetch them per recipient.

## Errors

| Status | Meaning | What to do |
| --- | --- | --- |
| 401 | API key is wrong | Send the raw key in `Authorization` with no `Bearer` prefix |
| 429 | Rate limited (`rate_limit_exceeded`) | Sleep until the `X-RateLimit-Reset` epoch second; there is no `Retry-After` |
| 503 | Maintenance | Retry later |
