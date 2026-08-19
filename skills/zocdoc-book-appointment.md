---
name: zocdoc-book-appointment
description: Use when booking a patient appointment through the Zocdoc partner API — searching the developer's provider directory by insurance, specialty or location, resolving real bookable timeslots, and creating the appointment. Covers the full search-to-booked flow against api-developer.zocdoc.com.
api: openapi/zocdoc-provider-locations-api-openapi.yml, openapi/zocdoc-providers-api-openapi.yml, openapi/zocdoc-appointments-api-openapi.yml
generated: '2026-08-15'
method: generated
source: openapi/ (v1.177) + https://api-docs.zocdoc.com/guides/patient/book-appointments
operations:
  - getProviderNpis
  - getProviders
  - getProviderLocations
  - getInsurancePlans
  - getVisitReasons
  - getProviderLocationsAvailability
  - createAppointment
  - getAppointment
---

# Book a Zocdoc appointment

This is the partner REST API at `https://api-developer.zocdoc.com`, not the public
zocdoc.com website. It requires partner credentials issued by Zocdoc. If you only need
to help a person find a doctor on the public site, use `zocdoc-doctor-finder` instead.

## Before you start

- **Get a token.** `POST https://auth.zocdoc.com/oauth/token` with
  `grant_type=client_credentials`, your `client_id`/`client_secret`, and
  `audience=https://api-developer.zocdoc.com/`. Send it as
  `Authorization: Bearer <token>`. Tokens live 60 minutes — cache and reuse one.
- **Match the environment.** A sandbox token against the production host returns a bare
  401 with no body. Sandbox token URL is
  `https://auth-api-developer-sandbox.zocdoc.com/oauth/token`, audience
  `https://api-developer-sandbox.zocdoc.com/`, base `https://api-developer-sandbox.zocdoc.com`.
- **Scopes.** Booking needs `external.appointment.write`. Reading needs
  `external.appointment.read`. Availability needs `external.schedulable_entity.read`.
- **Directory boundary.** You can only see providers in the directory Zocdoc defined
  jointly with your organisation. A provider outside it returns 404, not an empty result.

## Steps

### 1. Resolve the directory (cache this)

Call `getProviderNpis` (`GET /v1/reference/npi`) or `getSchedulableEntities`
(`GET /v1/schedulable_entities`) and cache the result. Zocdoc says this changes
infrequently — pull weekly, or after a known directory change. `getSchedulableEntities`
also accepts `recent_changes_72hrs=true` to fetch only what moved.

### 2. Find provider locations

Two entry points, pick by what you know:

- Know the doctor: `getProviders` (`GET /v1/providers?npis=...`) — up to 50 NPIs per
  call. Pass `latitude`+`longitude` together to get `distance` and `is_nearest_match`
  per location.
- Searching by need: `getProviderLocations` (`GET /v1/provider_locations`) with
  `zip_code` (required) plus one of `specialty_id` or `visit_reason_id`. Add
  `insurance_plan_id` to filter to in-network, `visit_type` for virtual vs in person,
  `max_distance_to_patient_mi` to tighten the radius (default 50).

Resolve ids first if you do not have them: `getInsurancePlans`
(`GET /v1/insurance_plans`), `getSpecialties` (`GET /v1/specialties`),
`getVisitReasons` (`GET /v1/visit_reasons`).

Both return `provider_location_id` in the composite form
`pr_<uuid>|lo_<uuid>`. **Pass it whole.** Splitting it is a client error.

### 3. Get real timeslots

`getProviderLocationsAvailability`
(`GET /v1/provider_locations/availability`). Required:
`provider_location_ids` (comma-delimited, max 50), `visit_reason_id`, `patient_type`
(`new` or `existing`). Visit reason and patient type are what determine appointment
duration, which is what turns raw calendar space into bookable slots — change either
and the slots change.

Optional window: `start_date_in_provider_local_time` /
`end_date_in_provider_local_time`, both `YYYY-MM-DD`. Defaults to today (US Eastern)
through +7 days. Maximum 150 days ahead, and a single request may span at most 31 days.

An empty `timeslots` array is a valid answer — that location has nothing bookable in
the window.

### 4. Book it

`createAppointment` (`POST /v1/appointments`) with `BookAppointmentRequestBody`:
`appointment_type` plus `data` (`AppointmentData`: `start_time`, `visit_reason_id`,
`provider_location_id`, `patient`, `patient_type`, optional `notes`).

Hard rules:

- **Only a `start_time` returned by step 3 is accepted.** Do not construct one.
- `notes` is capped at 100 characters (enforced since June 2026).
- Check `booking_requirements` on the provider location first. Some locations require
  an insurance member id and plan id; some refuse self-pay; some accept in-network
  plans only. Violating any of these returns 400.
- Set `developer_patient_id` if you have your own patient identifier — it is the only
  way to filter `getAppointments` by patient later.

### 5. Confirm the outcome

`createAppointment` returns an `appointment_id`. **The booking is not necessarily
complete.** Poll `getAppointment` (`GET /v1/appointments/{appointment_id}`) until the
status leaves `pending_booking`. Terminal-ish statuses are `confirmed` and
`booking_failed`.

## Rules that will bite you

- **There is no idempotency key.** None of these operations accept one. If
  `createAppointment` times out you do not know whether an appointment exists. Before
  retrying, call `getAppointments` filtered by `developer_patient_id` and
  `created_time_utc_min` and check. Never blind-retry a booking.
- **Webhooks will not tell you about your own actions.** For the patient booking use
  case Zocdoc sends `appointment_updated` only for *provider*-initiated changes. Update
  your state from the API response.
- **Time zones split two ways.** Availability is in provider-local time; appointment
  filters are UTC (`start_time_utc_min`/`max`). The `time_zone` field on location and
  availability objects is IANA (e.g. `America/New_York`) — use it, do not assume Eastern.
- **Errors carry no code.** You get an HTTP status plus
  `{request_id, error_type: api_error|invalid_request, errors:[{field, message}]}`.
  Branch on the status and `errors[].field`; log `request_id` for support. 401 responses
  have no body at all.
- **Rate limits are invisible.** 429 means back off exponentially. There is no
  `Retry-After` and no `RateLimit-*` header to read.
- **Credential scope matters.** A machine-to-machine credential can see and modify any
  appointment your organisation booked; a Zocdoc user credential can only touch that
  user's own appointments.

## Testing

Sandbox forces every outcome deterministically — see `sandbox/zocdoc-sandbox.yml`.
Book against `pr_confirmed|lo_confirmed` for a confirmed appointment,
`pr_bookingfailed|lo_bookingfailed` for a failure, `pr_error|lo_error` for a 500,
`insurance_plan_id=ip_0` for a 400.
