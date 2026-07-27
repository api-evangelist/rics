---
name: Validate a RICS survey-writer software licence
description: Check whether a survey-writing product licence key is valid for a RICS-regulated organisation and within its term — the one operation on this API built for third-party software vendors.
api: openapi/rics-digitalcommunity-api-openapi.json
operations:
  - POST /token
  - GET /api/SurveyWriter/{id}
generated: '2026-07-26'
method: generated
source: openapi/rics-digitalcommunity-api-openapi.json + data-model/rics-data-model.yml
---

# Validate a RICS survey-writer software licence

This is the one operation in the RICS DigitalCommunity API that clearly exists for a **third-party
software product** rather than for RICS's own back office: a survey-writing application asks RICS
whether the licence its user presented is valid.

Authenticate first with `POST /token` and send `Authorization: Bearer <token>` (see
`skills/rics-authenticate-and-read-regulation-scheme.md`). The OpenAPI declares no `operationId`s,
so the step is identified by method and path.

## The call

`GET https://api.rics.org/api/SurveyWriter/{id}`

`id` is a required string path parameter — the licence identifier your product holds.

## The response

`SurveyWriterModel`, a deliberately small object:

| Field | Meaning |
|---|---|
| `isValid` | The verdict. This is the field your product gates on. |
| `licenceKey` | The licence key the verdict applies to. |
| `organisation` | The RICS-regulated organisation the licence belongs to. |
| `productType` | The survey-writer product family. |
| `productCode` | The specific product. |
| `startDate` / `endDate` | The licence term. |

Gate on `isValid` **and** check `endDate` against the current date — do not infer validity from the
presence of a record.

`licenceKey` is the same concept the subscription side exposes as `SubscriptionUser.licenceKey`, so
a seat on a RICS subscription and a survey-writer licence are the same currency.

## Response codes

- `200` — a `SurveyWriterModel`. Read `isValid`.
- `401` — token missing or expired; re-run `POST /token`. There is no refresh grant.
- `403` — your RICS-issued credential is not entitled to check that licence.
- `404` — no such licence. Treat as **not valid**, and distinguish it from `isValid: false` in your
  own telemetry: `404` means "unknown key", `isValid: false` means "known key, not valid".

Note that the `401`, `403` and `404` responses on this operation declare **no response body** — the
status code is the entire signal. Only the `Payment`, `Profile` and `Regulation` operations bind
`ProblemDetails`.

## Operating notes

- **Cache carefully.** There are no cache headers, no ETag and no rate-limit headers on this API,
  and no published limits. Cache a positive verdict for a short window rather than calling on every
  user action, and always re-check after `endDate`.
- **No sandbox.** RICS publishes no test host, no test licence key and no test-mode flag on this
  operation, so there is no way to exercise the failure paths without real credentials and a real
  key.
- **Credentials are not self-serve.** A vendor wanting this integration must be issued a username
  and password by RICS; the published route is the
  [RICS Tech Partner Programme](https://www.rics.org/get-involved/rics-tech-partner-programme).
