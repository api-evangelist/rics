---
name: Authenticate with RICS and read a firm's regulation scheme
description: Exchange RICS-issued credentials for a bearer token and retrieve a regulated firm's licence record, including its offices, PII insurers, redress providers and annual returns.
api: openapi/rics-digitalcommunity-api-openapi.json
operations:
  - POST /token
  - GET /api/Regulation/{schemeNumber}
generated: '2026-07-26'
method: generated
source: openapi/rics-digitalcommunity-api-openapi.json + conventions/rics-conventions.yml
---

# Authenticate with RICS and read a firm's regulation scheme

The RICS DigitalCommunity API at `https://api.rics.org` is **not self-serve**. Before any of this
works, RICS must have issued you a username and password — the specification says so in its own
`info.description`. There is no signup form, no developer portal and no API key page. The only
published route to credentials is the [RICS Tech Partner
Programme](https://www.rics.org/get-involved/rics-tech-partner-programme).

The OpenAPI declares **no `operationId`s**, so every step below is identified by method and path.
`overlays/rics-digitalcommunity-api-overlay.yaml` carries the operationIds API Evangelist assigned
if you need names.

## 1. Get a bearer token

`POST https://api.rics.org/token`

Send a JSON object carrying the RICS-issued username and password. The specification models the
body only as a free-form object, so the exact property names come from RICS with your credentials —
do not guess them. Content type is `application/json`.

Rules:

- The response is a bearer JWT with a **limited, unpublished lifetime**. There is no refresh grant
  and no refresh endpoint: when the token expires, call `POST /token` again. The specification
  explicitly makes refresh "a matter for your client software to address".
- Treat `500` on this endpoint as a client-side body problem as well as a server fault. An empty
  JSON body returns `500` with a zero-length body, not the declared `400` (probed 2026-07-26).
- Never log the credential pair or the token.

## 2. Send the token on every subsequent call

Every operation is covered by one global security requirement: the `Bearer` scheme, declared as
`apiKey` in the `Authorization` header.

```
Authorization: Bearer <token>
```

Anonymous calls return `401` with `WWW-Authenticate: Bearer` and an empty body.

## 3. Read the regulation scheme

`GET https://api.rics.org/api/Regulation/{schemeNumber}`

`schemeNumber` is the firm's RICS regulation scheme number (path parameter, required, string).

The response is a single `RegulationScheme` — a wide, pre-expanded object. There is no field
expansion or sparse-fieldset parameter, so you always receive the whole record, including nested
`offices`, `memberDirectorPrincipals`, `piiInsurers`, `redressProviders`, `surveyingServices`,
`regulationReturns` and a self-referential `childSchemes` tree.

Read these first:

- `schemeLicenceStatus` / `schemeLicenceStatusName` — whether the licence is live.
- `schemeLicenceType` / `schemeLicenceTypeName` — what kind of regulation the firm holds.
- `hasPii` plus `piiInsurers` and `noPiiExplanation` — professional indemnity insurance position.
- `redressProviders` — the consumer redress scheme the firm belongs to.
- `licenceEndDate` and `licenceEndReason` — if the licence ended and why.
- `responsiblePrincipalId` / `responsiblePrincipalName` and `isResponsiblePrincipalValid`.
- `createdOn` / `modifiedOn` — the only change signal; there is no changelog and no ETag.

## 4. Handle the response codes correctly

- `200` — the scheme, as `RegulationScheme`.
- `204` — **found nothing**, not an error. This API returns `204` for an empty result rather than
  an empty body with `200`. Do not treat it as a failure.
- `400` — bad scheme number; body is a `ProblemDetails` object (`type`, `title`, `status`, `detail`,
  `instance`) served as `application/json`, not `application/problem+json`.
- `401` — token missing or expired; re-run step 1.
- `403` — your RICS-issued credential is not entitled to that record. There is no scope or
  permission model to inspect; contact RICS.

## 5. Things this API does not give you

Do not build these into your client, because they do not exist:

- No pagination — narrow by identifier only. Collections self-count instead
  (`countOfReturns`, `countOfMemberDirectorPrincipals`, `countOfSubscriptions`).
- No idempotency key on any write.
- No rate-limit headers, no `Retry-After`, and no published limits. Back off politely on your own.
- No request-id echo, so you cannot quote a correlation id when raising an incident.
- No status page, no SLA and no deprecation policy.

## 6. Data protection

`RegulationScheme` names individuals (licensed contacts, contact officers, responsible principals,
member directors and principals). This is personal data under UK GDPR. Store the minimum, do not
forward it to third-party models or logs, and do not cache it beyond what your agreement with RICS
permits.
