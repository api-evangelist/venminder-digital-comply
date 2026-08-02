---
generated: '2026-07-21'
method: generated
name: Review and set vendor risk
description: Pull a vendor's profile, contracts, documents, Venmonitor score and issues, then set its risk level.
api: openapi/venminder-digital-comply-openapi-original.json
operations:
  - GET /api/v1/ClientInformation/GetClientInformation
  - GET /api/v1/Vendors/GetVendorProfile
  - GET /api/v1/Contracts/GetContracts
  - GET /api/v1/Documents/GetDocumentsList
  - GET /api/v1/Venmonitor/GetVendorScore
  - GET /api/v1/Issues/GetAllIssues
  - POST /api/v1/Vendors/SetVendorRisk
source: >-
  Grounded in openapi/venminder-digital-comply-openapi-original.json (no operationIds in the
  spec; operations are METHOD + path, verified verbatim) and the Vendors/Venmonitor pages on
  developers.venminder.com.
---

# Review and set vendor risk

Assemble a vendor's full risk picture, then update its risk level.

## Auth
- Bearer token from the client-credentials grant (`scope=venminderApi`) at `https://login.venminder.com/connect/token`; base URL `https://rsd.venminder.com`. See `authentication/venminder-digital-comply-authentication.yml`.

## Steps
1. **Find the vendor** — `GET /api/v1/ClientInformation/GetClientInformation` lists the client's vendors and products with their keys.
2. **Pull the profile** — `GET /api/v1/Vendors/GetVendorProfile` for the vendor's detailed profile; `GET /api/v1/Vendors/ProfileAnswers` for profile question answers.
3. **Contracts** — `GET /api/v1/Contracts/GetContracts` (Ongoing or Offboarding) and `GET /api/v1/Contracts/GetDetails` for a specific contract.
4. **Documents** — `GET /api/v1/Documents/GetDocumentsList`, then `GET /api/v1/Documents/{documentKey}` to download.
5. **Continuous monitoring** — `GET /api/v1/Venmonitor/GetVendorScore` for the vendor's Venmonitor score (if licensed).
6. **Open issues** — `GET /api/v1/Issues/GetAllIssues`, drill in with `GET /api/v1/Issues/GetIssueDetails`.
7. **Set risk** — `POST /api/v1/Vendors/SetVendorRisk` to update the vendor's risk level, or `POST /api/v1/Vendors/Risk` for a product's risk level.

## Rules
- Writes are not idempotent; confirm state by re-reading the profile after a `POST`. See `conventions/venminder-digital-comply-conventions.yml`.
- Errors follow the `{statusCode, errors[]}` envelope in `errors/venminder-digital-comply-problem-types.yml`; `403` means the API key lacks the needed API permissions (permissions are chosen when the key is created in the Admin Panel).
