---
name: List team members and their devices
description: Page through a Dashlane team's members and resolve the activated devices for specific members.
api: openapi/dashlane-public-api-openapi-original.json
operations: [teams-Members, teams-MembersDeviceInformation]
method: generated
generated: '2026-07-18'
---

# List team members and their devices

Use the Dashlane Public API (read-only) to inventory team members and their devices.

## Auth
- HTTP bearer token: `Authorization: Bearer DLP_teamUuid_accessKey_secretKey` (Admin Console or `dcli team public-api create-key`).
- Base URL: `https://api.dashlane.com/public`.

## Steps
1. Page through members — `POST /teams/Members` (`teams-Members`) with body `{ "page": 0, "limit": 100, "order": "ASC", "orderBy": "<field>" }`. `page` is 0-based; increment until fewer than `limit` rows return. Collect member `email` values from `data`.
2. Resolve devices — `POST /teams/MembersDeviceInformation` (`teams-MembersDeviceInformation`) with body `{ "emails": ["a@corp.com", "b@corp.com"] }`. Read activated-device details from `data`.
3. Use `requestId` from each envelope for tracing/support.

## Rules
- Respect the page-number pagination (page/limit/order/orderBy) — do not assume cursors.
- Retry `500`/`503` with backoff; `400` indicates a malformed body or bad token.
- See conventions/dashlane-conventions.yml and errors/dashlane-problem-types.yml.
