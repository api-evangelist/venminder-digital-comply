---
generated: '2026-07-21'
method: generated
name: Onboard a new vendor
description: Create a vendor onboarding request in Venminder, then track it and read back its answers.
api: openapi/venminder-digital-comply-openapi-original.json
operations:
  - GET /api/v1/Vendor/Onboarding/Information
  - GET /api/v1/Vendor/Onboarding/FormDetails
  - POST /api/v1/Vendor/Onboarding/Request
  - GET /api/v1/Vendor/Onboarding/Requests
  - GET /api/v1/Vendor/Onboarding/RequestDetails
  - GET /api/v1/Vendor/Onboarding/RequestAnswers
source: >-
  Grounded in openapi/venminder-digital-comply-openapi-original.json (the spec declares no
  operationIds, so operations are METHOD + path, verified verbatim in the spec) and the
  Vendor Onboarding pages on developers.venminder.com.
---

# Onboard a new vendor

Create and track a vendor onboarding request on the Venminder platform.

## Auth
- OAuth 2.0 client credentials: `POST https://login.venminder.com/connect/token` with `grant_type=client_credentials&scope=venminderApi`, HTTP Basic `<api_key>:<client_secret>`. Send the returned token as `Authorization: Bearer <access_token>` on every call to `https://rsd.venminder.com`. See `authentication/venminder-digital-comply-authentication.yml`.

## Steps
1. **Read onboarding configuration** — `GET /api/v1/Vendor/Onboarding/Information` to learn the client's onboarding setup (available forms/workflow).
2. **Fetch the form** — `GET /api/v1/Vendor/Onboarding/FormDetails` for the specific onboarding request form and its questions.
3. **(Optional) Fetch expected answers** — `GET /api/v1/Vendor/Onboarding/FormAnswers` to see the expected answers for form questions.
4. **Create the request** — `POST /api/v1/Vendor/Onboarding/Request` with the form answers. The same endpoint updates an existing request.
5. **Track it** — `GET /api/v1/Vendor/Onboarding/Requests` to list requests, then `GET /api/v1/Vendor/Onboarding/RequestDetails` for one request.
6. **Read/edit answers** — `GET /api/v1/Vendor/Onboarding/RequestAnswers`; correct with `POST /api/v1/Vendor/Onboarding/EditRequestAnswers`.

## Rules
- No idempotency-key mechanism exists — do not blind-retry the `POST` calls; re-read `Requests` first to confirm whether the request landed. See `conventions/venminder-digital-comply-conventions.yml`.
- Validation failures return `{statusCode: 400, errors: [{resourceKey, resourceValue, field}]}` — map `field` back to the form question. `401` means the bearer token expired: re-run the token call. See `errors/venminder-digital-comply-problem-types.yml`.
