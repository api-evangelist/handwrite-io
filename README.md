# Handwrite IO

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Handwrite is a South Carolina based direct-mail service that writes and mails real handwritten cards on demand, and exposes that fulfillment as a small REST API at https://api.handwrite.io/v1. Four operations cover the whole surface: list the handwriting styles on the account, list the stationery and card options, send a card to between one and ten US recipients (or up to 1,000 orders in a single batch request), and fetch an order to follow it from processing through written to complete and retrieve proof images of the finished card and envelope. Authentication is a single static API key sent raw in the Authorization header, prefixed test_hw for unbilled test-mode calls or live_hw for calls that actually mail and bill. Pricing is per mailed card rather than per API call, from $2.99 down to $2.45 by volume, with the card, envelope, handwriting and postage included. The API is rate limited to 60 requests per minute per key. Handwrite also ships a Zapier integration for teams that do not want to write code.

**Areas:** Direct Mail · Handwritten · Handwritten Notes · Cards · Marketing · Notes · Print · Fulfillment · Customer Engagement

## Handwrite IO on APIs.io

Everything profiled for Handwrite IO is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/handwrite-io/](https://apis.io/providers/handwrite-io/)** — or go straight to one of the APIs or properties below.

### APIs (4)

- [Handwrite IO Handwriting API](https://apis.io/apis/handwrite-io/handwrite-io-handwriting-api/) — Available handwriting styles
- [Handwrite IO Orders API](https://apis.io/apis/handwrite-io/handwrite-io-orders-api/) — Order tracking and status
- [Handwrite IO Send API](https://apis.io/apis/handwrite-io/handwrite-io-send-api/) — Send handwritten notes
- [Handwrite IO Stationery API](https://apis.io/apis/handwrite-io/handwrite-io-stationery-api/) — Available stationery and cards

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Handwrite Io Agentic Access](https://apis.io/agentic-access/handwrite-io/handwrite-io-agentic-access/)

#### Collections (5)

Postman and Open Collection request collections.

- [Handwrite IO API](https://apis.io/collections/handwrite-io/open-handwrite-io/)
- [Handwrite IO Handwriting API](https://apis.io/collections/handwrite-io/open-handwrite-io-handwriting-api/)
- [Handwrite IO Handwriting Orders API](https://apis.io/collections/handwrite-io/open-handwrite-io-orders-api/)
- [Handwrite IO Handwriting Send API](https://apis.io/collections/handwrite-io/open-handwrite-io-send-api/)
- [Handwrite IO Handwriting Stationery API](https://apis.io/collections/handwrite-io/open-handwrite-io-stationery-api/)

#### Overlays (4)

OpenAPI overlays, including conversational phrasing for each operation.

- [Handwrite Io Handwriting Api Overlay](https://apis.io/overlays/handwrite-io/handwrite-io-handwriting-api-overlay/)
- [Handwrite Io Orders Api Overlay](https://apis.io/overlays/handwrite-io/handwrite-io-orders-api-overlay/)
- [Handwrite Io Send Api Overlay](https://apis.io/overlays/handwrite-io/handwrite-io-send-api-overlay/)
- [Handwrite Io Stationery Api Overlay](https://apis.io/overlays/handwrite-io/handwrite-io-stationery-api-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Handwrite IO API Rules](https://apis.io/rules/handwrite-io/handwrite-io-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Handwrite Io Authentication](https://apis.io/security/handwrite-io/handwrite-io-authentication/)
- [Handwrite Io Domain Security](https://apis.io/security/handwrite-io/handwrite-io-domain-security/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Handwrite Io Plans Pricing](https://apis.io/plans/handwrite-io/handwrite-io-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Handwrite Io Rate Limits](https://apis.io/rate-limits/handwrite-io/handwrite-io-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Handwrite Io Finops](https://apis.io/finops/handwrite-io/handwrite-io-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://handwrite.io/)
- [Documentation](https://documentation.handwrite.io/)
- [Getting started](https://documentation.handwrite.io/#getting-started)
- [API reference](https://documentation.handwrite.io/#endpoints)
- [Pricing](https://www.handwrite.io/pricing)
- [Sign up](https://app.handwrite.io/signup)
- [Support](https://www.handwrite.io/contact-us)
- [Blog](https://www.handwrite.io/blog)
- [GitHub](https://github.com/handwriteio)

## API Evangelist rating

**Kin Score 45.0** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 66.1 |
| Contract quality | 50.2 |
| Contract governance | 22.0 |
| Operational transparency | 23.7 |
| Developer ergonomics | 51.8 |
| Access clarity | 55.3 |

**Agent readiness 22.7** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | na |

**Regulatory alignment 15.2** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/handwrite-io/](https://apis.io/providers/handwrite-io/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Handwrite IO: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/handwrite-io/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
