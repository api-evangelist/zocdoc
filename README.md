# Zocdoc

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

Zocdoc is a healthcare appointment booking platform that provides a REST API for accessing provider availability, booking appointments, managing insurance verification, and patient scheduling. The Zocdoc for Developers platform enables integration with Zocdoc's provider network through patient booking, provider calendar integration, and insurance APIs using OAuth 2.0 authentication with both sandbox and production environments.

**Areas:** Healthcare · Appointments · Booking · Providers · Insurance · Telehealth · Scheduling

## Zocdoc on APIs.io

Everything profiled for Zocdoc is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/zocdoc/](https://apis.io/providers/zocdoc/)** — or go straight to one of the APIs or properties below.

### APIs (13)

- [Zocdoc Appointments API](https://apis.io/apis/zocdoc/zocdoc-appointments-api/) — Endpoints for booking, cancelling, and rescheduling appointments, including retrieving current appointment statuses and updated information.
- [Zocdoc Calendar Integration Timeslots API](https://apis.io/apis/zocdoc/zocdoc-calendar-integration-timeslots-api/) — Endpoints to manage timeslots for providers.
- [Zocdoc Credentials API](https://apis.io/apis/zocdoc/zocdoc-credentials-api/) — Endpoints for managing API credentials.
- [Zocdoc Facilities API](https://apis.io/apis/zocdoc/zocdoc-facilities-api/) — Endpoints to retrieve facilities within the developer's directory.
- [Zocdoc Insurance Reference API](https://apis.io/apis/zocdoc/zocdoc-insurance-reference-api/) — Endpoints to retrieve insurance plans supported by Zocdoc.
- [Zocdoc Provider Locations API](https://apis.io/apis/zocdoc/zocdoc-provider-locations-api/) — Endpoints for retrieving and modifying provider location objects and their related insurance plans and availability.
- [Zocdoc Providers API](https://apis.io/apis/zocdoc/zocdoc-providers-api/) — Endpoints to retrieve providers within the developer's directory.
- [Zocdoc Reference API](https://apis.io/apis/zocdoc/zocdoc-reference-api/) — Endpoints to retrieve information about the developer's directory.
- [Zocdoc Schedulable Entities API](https://apis.io/apis/zocdoc/zocdoc-schedulable-entities-api/) — Endpoints to retrieve schedulable entities with availability information.
- [Zocdoc Webhook API](https://apis.io/apis/zocdoc/zocdoc-webhook-api/) — Sandbox endpoints to mock webhook behavior
- [Zocdoc Reviews API](https://apis.io/apis/zocdoc/zocdoc-reviews-api/) — Endpoints to retrieve aggregate review summaries for providers, individually or in batches of up to 100 provider IDs.
- [Zocdoc Specialties API](https://apis.io/apis/zocdoc/zocdoc-specialties-api/) — Reference endpoints to retrieve the specialties Zocdoc supports and their default visit reasons, optionally filtered by care category.
- [Zocdoc Visit Reasons API](https://apis.io/apis/zocdoc/zocdoc-visit-reasons-api/) — Reference endpoints to retrieve the visit reasons Zocdoc supports and the specialty each one belongs to. Visit reason drives appointment duration and…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Zocdoc Webhooks](https://apis.io/asyncapis/zocdoc/zocdoc-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [zocdoc](https://apis.io/servers/zocdoc/zocdoc/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Zocdoc Agentic Access](https://apis.io/agentic-access/zocdoc/zocdoc-agentic-access/)

#### JSON Schema (3)

The data models the API reads and writes.

- [Appointment](https://apis.io/schemas/zocdoc/zocdoc-appointment/)
- [Availability](https://apis.io/schemas/zocdoc/zocdoc-availability/)
- [Provider](https://apis.io/schemas/zocdoc/zocdoc-provider/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Zocdoc Context](https://apis.io/jsonld/zocdoc/zocdoc-context/)

#### Vocabulary (1)

The provider's domain terms, defined.

- [Zocdoc API Vocabulary](https://apis.io/vocabularies/zocdoc/zocdoc-vocabulary/)

#### Examples (3)

Example requests and responses.

- [Availability Response](https://apis.io/examples/zocdoc/availability-response/)
- [Book Appointment Request](https://apis.io/examples/zocdoc/book-appointment-request/)
- [Book Appointment Response](https://apis.io/examples/zocdoc/book-appointment-response/)

#### Collections (10)

Postman and Open Collection request collections.

- [API Documentation appointments API](https://apis.io/collections/zocdoc/open-zocdoc-appointments-api/)
- [API Documentation appointments calendar-integration-timeslots API](https://apis.io/collections/zocdoc/open-zocdoc-calendar-integration-timeslots-api/)
- [API Documentation appointments credentials API](https://apis.io/collections/zocdoc/open-zocdoc-credentials-api/)
- [API Documentation appointments facilities API](https://apis.io/collections/zocdoc/open-zocdoc-facilities-api/)
- [API Documentation appointments insurance-reference API](https://apis.io/collections/zocdoc/open-zocdoc-insurance-reference-api/)
- [API Documentation appointments provider-locations API](https://apis.io/collections/zocdoc/open-zocdoc-provider-locations-api/)
- [API Documentation appointments providers API](https://apis.io/collections/zocdoc/open-zocdoc-providers-api/)
- [API Documentation appointments reference API](https://apis.io/collections/zocdoc/open-zocdoc-reference-api/)
- [API Documentation appointments schedulable-entities API](https://apis.io/collections/zocdoc/open-zocdoc-schedulable-entities-api/)
- [API Documentation appointments webhook API](https://apis.io/collections/zocdoc/open-zocdoc-webhook-api/)

#### Overlays (13)

OpenAPI overlays, including conversational phrasing for each operation.

- [Zocdoc Appointments Overlay](https://apis.io/overlays/zocdoc/zocdoc-appointments-overlay/)
- [Zocdoc Calendar Integration Timeslots Overlay](https://apis.io/overlays/zocdoc/zocdoc-calendar-integration-timeslots-overlay/)
- [Zocdoc Credentials Overlay](https://apis.io/overlays/zocdoc/zocdoc-credentials-overlay/)
- [Zocdoc Facilities Overlay](https://apis.io/overlays/zocdoc/zocdoc-facilities-overlay/)
- [Zocdoc Insurance Reference Overlay](https://apis.io/overlays/zocdoc/zocdoc-insurance-reference-overlay/)
- [Zocdoc Provider Locations Overlay](https://apis.io/overlays/zocdoc/zocdoc-provider-locations-overlay/)
- [Zocdoc Providers Overlay](https://apis.io/overlays/zocdoc/zocdoc-providers-overlay/)
- [Zocdoc Reference Overlay](https://apis.io/overlays/zocdoc/zocdoc-reference-overlay/)
- [Zocdoc Reviews Overlay](https://apis.io/overlays/zocdoc/zocdoc-reviews-overlay/)
- [Zocdoc Schedulable Entities Overlay](https://apis.io/overlays/zocdoc/zocdoc-schedulable-entities-overlay/)
- [Zocdoc Specialties Overlay](https://apis.io/overlays/zocdoc/zocdoc-specialties-overlay/)
- [Zocdoc Visit Reasons Overlay](https://apis.io/overlays/zocdoc/zocdoc-visit-reasons-overlay/)
- [Zocdoc Webhook Overlay](https://apis.io/overlays/zocdoc/zocdoc-webhook-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Zocdoc — jsonschema rules](https://apis.io/rules/zocdoc/zocdoc-jsonschema-spectral-rules/)

#### Security (3)

Authentication, trust centre, and vulnerability disclosure.

- [Zocdoc Authentication](https://apis.io/security/zocdoc/zocdoc-authentication/)
- [Zocdoc Domain Security](https://apis.io/security/zocdoc/zocdoc-domain-security/)
- [Zocdoc Vulnerability Disclosure](https://apis.io/security/zocdoc/zocdoc-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Zocdoc Scopes](https://apis.io/scopes/zocdoc/zocdoc-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Zocdoc Plans Pricing](https://apis.io/plans/zocdoc/zocdoc-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Zocdoc Rate Limits](https://apis.io/rate-limits/zocdoc/zocdoc-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Zocdoc Finops](https://apis.io/finops/zocdoc/zocdoc-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.zocdoc.com)
- [Documentation](https://api-docs.zocdoc.com/guides)
- [Getting started](https://api-docs.zocdoc.com/guides)
- [API reference](https://api-docs.zocdoc.com/apis)
- [Pricing](https://www.zocdoc.com/about/news/zocdoc-launches-its-first-ever-public-api-platform-zocdoc-for-developers/)
- [Sign up](https://developer.zocdoc.com/#api-form)
- [Support](https://api-docs.zocdoc.com/guides/faqs)
- [Blog](https://medium.com/zocdoc-engineering)
- [Terms of service](https://www.zocdoc.com/about/terms)
- [Privacy policy](https://www.zocdoc.com/about/consumer-health-data-privacy-policy/)

## API Evangelist rating

**Kin Score 58.3** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 73.2 |
| Contract quality | 64.4 |
| Contract governance | 28.0 |
| Operational transparency | 28.9 |
| Developer ergonomics | 64.3 |
| Access clarity | 59.2 |

**Agent readiness 50.1** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | documented (partial credit) |
| MCP Server | — |
| Auth Clarity | served (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | documented (partial credit) |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | yes |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | — |
| Dynamic Client Registration | yes |
| Agentic Commerce | — |
| Pagination Ratio | na |
| Operation Distinctiveness | distinct |

**Regulatory alignment 37.5** — Health

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/zocdoc/](https://apis.io/providers/zocdoc/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Zocdoc: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/zocdoc/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
