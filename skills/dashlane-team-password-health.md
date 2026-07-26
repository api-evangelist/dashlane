---
name: Audit team password health
description: Retrieve a Dashlane team's status and password-health metrics to report on weak, reused, and compromised credentials.
api: openapi/dashlane-public-api-openapi-original.json
operations: [teams-Status, teams-PasswordHealth]
method: generated
generated: '2026-07-18'
---

# Audit team password health

Use the Dashlane Public API (read-only) to report on a team's credential hygiene.

## Auth
- All `/teams/*` calls require an HTTP bearer token: `Authorization: Bearer DLP_teamUuid_accessKey_secretKey`.
- Generate the key in the Dashlane Admin Console or with `dcli team public-api create-key`.
- Base URL: `https://api.dashlane.com/public`.

## Steps
1. Confirm the team is active — `POST /teams/Status` (`teams-Status`). Read `data` from the `{ requestId, data }` envelope.
2. Pull password-health metrics — `POST /teams/PasswordHealth` (`teams-PasswordHealth`) with body `{ "numberOfDays": <window> }`. Read `data` for weak/reused/compromised counts.
3. Correlate any support follow-up using the `requestId` returned in each response.

## Rules
- Retry `500`/`503` with exponential backoff; check https://status.dashlane.com on repeated `503`.
- A `400` means the request body/Authorization header is malformed — verify `numberOfDays` and the DLP token.
- See conventions/dashlane-conventions.yml and errors/dashlane-problem-types.yml.
