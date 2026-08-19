---
name: zocdoc-sync-insurance-offerings
description: Use when reading or updating which insurance plans a Zocdoc provider location accepts — resolving Zocdoc insurance plan ids, reading current mappings, and submitting asynchronous mapping changes. The Zocdoc Insurance API is restricted to select partners.
api: openapi/zocdoc-insurance-reference-api-openapi.yml, openapi/zocdoc-provider-locations-api-openapi.yml
generated: '2026-08-15'
method: generated
source: openapi/ (v1.177) + https://api-docs.zocdoc.com/guides/insurance/update-insurance
operations:
  - getInsurancePlans
  - getInsurancePlan
  - getProviderLocationInsuranceMappings
  - updateProviderLocationInsuranceMappings
---

# Sync insurance offerings

Access note: Zocdoc states the Insurance API is **for select partners only**. If your
client is not enabled for it, `updateProviderLocationInsuranceMappings` returns 403
regardless of token validity. Writes need `external.provider_insurance.write`.

## 1. Resolve Zocdoc plan ids

`getInsurancePlans` (`GET /v1/insurance_plans`) is the authoritative lookup — do not
map carrier names by hand. Filters: `status` (default `active`), `state` (two-letter),
`network_type`, `program_type`, `care_category`. Paging: `page` 0–200, `page_size`
default 100, max 500.

`getInsurancePlan` (`GET /v1/insurance_plans/{insurance_plan_id}`) fetches one.

Ids are prefixed `ip_`. Plan types now include `hmo_pos`, `medicare_advantage`,
`medicaid_managed_care` and `federal` (added May 2026). Each plan carries `carrier`,
`network_type`, `program_type`, `status`, `care_categories` and `coverage_area`.

## 2. Read the current mappings

`getProviderLocationInsuranceMappings`
(`GET /v1/provider_locations/{provider_location_id}/insurance_mappings`) returns both
the eligible plans and the current mappings for that location.

Remember `provider_location_id` is the composite `pr_<uuid>|lo_<uuid>` — pass it whole.

A **409 Conflict** here means the mappings for this provider location are managed at
the **practice** or **provider** level, not the location level. That is a modelling
answer, not a transient failure: you cannot write location-level mappings for it.

## 3. Submit changes

`updateProviderLocationInsuranceMappings`
(`POST /v1/provider_locations/{provider_location_id}/insurance_mappings`) with
`UpdateInsuranceMappingsRequest` — a list of `InsuranceMappingUpdateOperation` entries.

**This is asynchronous.** A `202 Accepted` means the request was accepted for
processing, **not** that the mapping changed. Do not treat 202 as success:

1. Record the returned `request_id`.
2. Re-read `getProviderLocationInsuranceMappings` on a later pass to confirm the state
   actually converged.
3. Do not immediately re-submit on a slow apply — there is no idempotency key, so a
   duplicate submission is a genuinely duplicate request.

Expect 409 on the same practice/provider-level condition as step 2, and 400 with
`errors[].field` for an invalid `ip_` id.

## Rules that will bite you

- **202 is not done.** This is the only operation in the Zocdoc API that is explicitly
  asynchronous, and the most common integration bug is treating it as complete.
- **No idempotency key**, so build convergence-by-re-read rather than retry-on-timeout.
- **Insurance drives discovery.** `insurance_plan_id` filters `getProviderLocations`,
  `getProviders` and `getProviderLocationsAvailability`, and a location's
  `booking_requirements` may demand an insurance member id, may refuse self-pay, or may
  accept in-network plans only. Wrong mappings surface downstream as 400s on booking,
  not as errors here.
- Errors carry no code — branch on status plus `errors[].field`, log `request_id`.

## Testing

Sandbox triggers (see `sandbox/zocdoc-sandbox.yml`):
`pr_allInsurancePlansAccep|lo_allInsurancePlansAccep` returns full mappings and 202 on
update; `pr_noAcceptedInsurancePla|lo_noAcceptedInsurancePla` returns empty mappings;
`pr_ip_mapping_level_prac|lo_ip_mapping_level_prac` and
`pr_ip_mapping_level_prov|lo_ip_mapping_level_prov` both return 409;
`pr_missing|lo_missing` returns 404; `ip_0` is the invalid plan id;
`state=AK` returns an empty plan list.
