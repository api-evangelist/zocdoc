---
name: zocdoc-publish-provider-availability
description: Use when writing provider availability into Zocdoc from a practice management or EHR system — publishing and overwriting calendar timeslots for a provider on a date, reconciling the schedulable-entity directory, and submitting NPI overlaps. The provider-side (calendar integration) half of the Zocdoc partner API.
api: openapi/zocdoc-calendar-integration-timeslots-api-openapi.yml, openapi/zocdoc-schedulable-entities-api-openapi.yml
generated: '2026-08-15'
method: generated
source: openapi/ (v1.177) + https://api-docs.zocdoc.com/guides/scheduling/create-timeslots
operations:
  - getSchedulableEntities
  - putSchedulableEntitiesOverlaps
  - getProviderCalendarTimeslots
  - putProviderCalendarTimeslots
---

# Publish provider availability to Zocdoc

This is the calendar integration use case: your system owns the schedule, Zocdoc
publishes it. Auth and error handling are as in `zocdoc-book-appointment`.

## 1. Know which providers you may write for

`getSchedulableEntities` (`GET /v1/schedulable_entities`) returns the entities in your
directory with availability metadata: `id`, `npi`, `type`, `time_zone` (IANA),
`main_specialty_id` / `main_specialty_name`, `go_live_timestamp_utc`,
`profile_last_modified_timestamp_utc`, and new/existing patient availability info.

- `page_size` defaults to **5,000** and maxes at **10,000** (both reduced in June 2026 —
  if your integration hardcoded 60,000 it now fails validation).
- `recent_changes_72hrs=true` plus the `recent_change_summary` field gives you a delta
  instead of a full pull.
- Zocdoc's guidance: cache this, refresh weekly or after a known directory change.

`putSchedulableEntitiesOverlaps` (`PUT /v1/schedulable_entities/overlaps`) submits NPIs
to check whether they are schedulable through you. The response carries a
`ZocdocSchedulableStatus` of `schedulable`, `not_schedulable` or `added_to_queue`.

## 2. Read what Zocdoc currently holds

`getProviderCalendarTimeslots`
(`GET /v1/providers/{provider_id}/calendar/timeslots?date=YYYY-MM-DD`).

The `date` matches the **local date part of the slot's `start_time`**, not UTC. A slot
at `2024-10-01T22:00` in `America/New_York` belongs to `2024-10-01` here even though it
is `2024-10-02` in UTC. Getting this wrong silently returns the wrong day.

Paginates by cursor: `limit` plus `next_page_token`. Follow `next_page_token` until it
is null; do not count items against `limit`.

## 3. Write the day

`putProviderCalendarTimeslots`
(`PUT /v1/providers/{provider_id}/calendar/timeslots`).

**This is a full replacement for one provider on one date.** Three consequences you
must design for:

1. **Send the entire set of slots for that date, every time.** A partial array is not a
   partial update — it is the new complete truth.
2. **A second request for the same provider and date overwrites the first.** Do not
   append.
3. **An empty array deletes every slot for that provider on that date, across all
   locations.** This is the intended way to clear a day, and it is also the easiest way
   to wipe a schedule by accident. Guard it.

Multiple locations on one date means multiple timeslot objects in the same array — one
per location.

Since May 2026 the response body carries error detail when location ids cannot be
resolved. Read it; do not assume a 2xx means every slot landed.

## 4. Expect eventual consistency

Zocdoc stores timeslots in an eventually consistent data store. A `GET` immediately
after a `PUT` may not reflect the write. Do not use read-after-write as your
confirmation; treat the `PUT` response as the acknowledgement and reconcile on your
next scheduled sync.

## Rules that will bite you

- **No idempotency key** — but the full-replace semantics make `PUT` naturally
  idempotent per (provider, date). That is your retry safety here, and it is the only
  place in this API you get any.
- **Time zone is the main hazard.** Every provider carries a `time_zone` IANA
  identifier; use it to compute the local date before choosing which day to write.
- **Bookings arrive as webhooks.** In the provider scheduling use case
  `appointment_update_type` can be `created` — this is the only use case where that
  value appears. Verify the HMAC-SHA256 `webhook-signature` over
  `<webhook-timestamp>.<payload>` and reject timestamps more than 5 minutes old. See
  `asyncapi/zocdoc-webhooks.yml`.
- **429 has no header to read.** Back off exponentially.

## Testing

Sandbox NPI triggers for overlaps: `npi_overlaps_not_found` returns
`NotSchedulable`, `npi_overlaps_pending` returns `AddedToQueue`. Use
`mockWebhookRequest` (`POST /v1/webhook/mock-request`) to drive a signed webhook at
your receiver — sandbox is the only environment that accepts a plain-HTTP receiver and
an arbitrary base64 signing key.
