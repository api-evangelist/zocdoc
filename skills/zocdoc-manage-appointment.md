---
name: zocdoc-manage-appointment
description: Use when changing an existing Zocdoc appointment through the partner API — confirming, cancelling, rescheduling, marking arrived or no-show, uploading attachments, or reading appointment status and participants. Covers the appointment state machine and the 409 conflicts it raises.
api: openapi/zocdoc-appointments-api-openapi.yml
generated: '2026-08-15'
method: generated
source: openapi/ (v1.177) + https://api-docs.zocdoc.com/guides/scheduling/appointment-actions
operations:
  - getAppointment
  - getAppointments
  - getAppointmentParticipants
  - confirmAppointment
  - cancelAppointment
  - rescheduleAppointment
  - updateAppointmentStatus
  - uploadAppointmentAttachment
---

# Manage a Zocdoc appointment

Auth, environment and error rules are the same as `zocdoc-book-appointment`. Every
operation here needs `external.appointment.read` or `external.appointment.write`.

## The state machine

`AppointmentStatus` is one of: `pending_booking`, `confirmed`, `booking_failed`,
`cancelled`, `no_show`, `pending_reschedule`, `rescheduled`, `reschedule_failed`.

Read the current status with `getAppointment`
(`GET /v1/appointments/{appointment_id}`) **before** attempting any transition.
Attempting a transition the current status does not permit returns **409 Conflict** —
that is the signal, and it is not retryable without re-reading state.

## Transitions

| Intent | Operation | Path | Notes |
|---|---|---|---|
| Confirm a pending booking | `confirmAppointment` | `POST /v1/appointments/confirm` | Only valid from `pending_booking`. |
| Cancel | `cancelAppointment` | `POST /v1/appointments/cancel` | Valid from any non-cancelled status: `pending_booking`, `booking_failed`, `confirmed`, `pending_reschedule`, `reschedule_failed`, `rescheduled`. |
| Move the time | `rescheduleAppointment` | `POST /v1/appointments/reschedule` | Valid from `pending_booking`, `confirmed`, `pending_reschedule`, `rescheduled`. Time only — nothing else about the appointment changes. |
| Patient arrived / did not show | `updateAppointmentStatus` | `PUT /v1/appointments/update-status` | `arrived` or `no_show` only. `no_show` requires a start time in the past and no older than 2 days. |

### Cancelling well

`CancelAppointmentRequestBody` takes `appointment_id` (required) plus an optional
`cancellation_reason_type` enum. Send the enum whenever you know the reason — Zocdoc
uses it for metrics. Only set the free-text `cancellation_reason` when the type is
`other_patient_reason` or `other_provider_reason`.

### Rescheduling well

You must supply a new `start_time`, and the same rule as booking applies: **it must be a
slot returned by `getProviderLocationsAvailability`**. Re-query availability for the
same `provider_location_id` / `visit_reason_id` / `patient_type` before rescheduling.

## Reading appointments

`getAppointments` (`GET /v1/appointments`) is paginated and filterable:

- Paging: `page` 0–10, `page_size` 1–100. **Hard ceiling of 1,000 appointments across
  all pages** — if you expect more, narrow the filters and batch.
- Filters: `statuses` (comma-delimited), `developer_patient_id`, `practice_ids`,
  `provider_ids`, `location_ids`, and four UTC time windows —
  `start_time_utc_min`/`max`, `created_time_utc_min`/`max`,
  `last_modified_time_utc_min`/`max`.
- Sorting: `sort_by` (default `start_time`), `sort_direction` (default descending).

Use `last_modified_time_utc_min` to build a delta sync rather than re-reading
everything.

`getAppointmentParticipants` (`GET /v1/appointments/{appointment_id}/participants`)
returns who is on the appointment.

**Credential visibility:** a machine-to-machine credential sees every appointment your
organisation booked; a Zocdoc user credential sees only that user's own appointments.

## Attachments

`uploadAppointmentAttachment`
(`POST /v1/appointments/{appointment_id}/attachments`), `multipart/form-data`.

- Types: jpg, png, pdf, docx only.
- Max 100 MB (raised from 10 MB in May 2026).
- Attachment types include `patient_front_insurance_card` and
  `patient_back_insurance_card` (added May 2026).
- Multiple attachments of the same type on one appointment are allowed.
- **Rejected** for appointments in a failed state, or more than 7 days after the
  appointment.

## Rules that will bite you

- **No idempotency key on any of these.** Cancel and confirm are naturally idempotent
  in effect (a second call 409s), but reschedule and attachment upload are not — a
  retried upload creates a second attachment. Read state before retrying.
- **409 means read, then decide.** It is a state-machine violation, not a transient
  error. Backing off and retrying the same call will fail the same way.
- **Webhooks only fire for provider-initiated changes** in the patient booking use
  case. Your own cancel/reschedule will not produce a webhook — take the API response
  as truth.
- Log the `request_id` from every response body. It is the only correlation handle
  Zocdoc offers, and it is absent from bare 401s.

## Testing

Sandbox publishes fixed appointment UUIDs per status (see
`sandbox/zocdoc-sandbox.yml`) — e.g. `2b29f79b-6d7f-472a-9603-d0c378bc9531` is always
`pending_booking`, `21990114-ea71-4d7d-9d1e-00c43ae44bcd` is always `cancelled`,
`83f5cf14-3eb1-4034-be1f-e7c3058aad21` always 404s and
`dc69a428-8a73-461b-bd7b-df755910a3fb` always 500s.
