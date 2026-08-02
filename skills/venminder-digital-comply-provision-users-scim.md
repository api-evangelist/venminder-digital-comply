---
generated: '2026-07-21'
method: generated
name: Provision platform users via SCIM 2.0
description: Create, search, update and deactivate Venminder platform users through the SCIM 2.0 surface.
api: https://rsd.venminder.com/scim/v2
operations:
  - GET /scim/v2/Users
  - GET /scim/v2/Users/[id]
  - POST /scim/v2/Users/.search
  - POST /scim/v2/Users/
  - PATCH /scim/v2/Users/[id]
source: >-
  Grounded in the USER PROVISIONING pages on developers.venminder.com and the official
  Postman collection (postman/venminder-digital-comply-postman-collection.json, "SCIM 2.0"
  folder). The SCIM surface is not part of the Swagger document.
---

# Provision platform users via SCIM 2.0

Manage Venminder platform users with standard SCIM 2.0 calls.

## Auth
- Same OAuth 2.0 client-credentials bearer token as the REST API (`scope=venminderApi`), sent to `https://rsd.venminder.com/scim/v2/...`. See `authentication/venminder-digital-comply-authentication.yml`.

## Steps
1. **List users** — `GET /scim/v2/Users`; filter with SCIM syntax, e.g. `GET /scim/v2/Users?filter=userName eq "name@domain.com"`. Responses are `urn:ietf:params:scim:api:messages:2.0:ListResponse` with `totalResults`/`startIndex`/`itemsPerPage`.
2. **Fetch one** — `GET /scim/v2/Users/[id]`.
3. **Search** — `POST /scim/v2/Users/.search` with a SCIM search request body.
4. **Create** — `POST /scim/v2/Users/` with schemas `urn:ietf:params:scim:schemas:core:2.0:User` (userName is the email) and optionally the `urn:ietf:params:scim:schemas:extension:Venminder:2.0:User` extension (e.g. `enrollmentDate`).
5. **Update** — `PATCH /scim/v2/Users/[id]` with a `urn:ietf:params:scim:api:messages:2.0:PatchOp` body (`op: replace` operations; set `active: false` to deactivate).

## Rules
- Errors are standard SCIM: `urn:ietf:params:scim:api:messages:2.0:Error` with `detail` and `status` — `404` "User not found.", `409` "User already exists in the database." (treat 409 on create as already-provisioned, not a failure).
- See `errors/venminder-digital-comply-problem-types.yml` (scim_errors) and `conventions/venminder-digital-comply-conventions.yml`.
