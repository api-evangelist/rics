---
name: Check a RICS firm's subscriptions and billing position
description: Read the subscriptions, licence seats, quotes, sales orders and payment requests attached to a RICS regulation scheme, without moving any money.
api: openapi/rics-digitalcommunity-api-openapi.json
operations:
  - POST /token
  - GET /api/Regulation/Subscriptions
  - GET /api/Regulation/PaymentInformation
  - GET /api/Payment/{id}
  - GET /api/Payment/{id}/reference/{reference}
generated: '2026-07-26'
method: generated
source: openapi/rics-digitalcommunity-api-openapi.json + data-model/rics-data-model.yml
---

# Check a RICS firm's subscriptions and billing position

This is the **read-only** half of the RICS commercial surface. It answers: what is this regulated
firm subscribed to, who holds the licence seats, what has been quoted and ordered, and what is
outstanding. Everything here is a `GET`. Writing — `POST /api/Payment/update` — is deliberately out
of scope for this skill; see the warning at the end.

Authenticate first with `POST /token` and send `Authorization: Bearer <token>` on every call (see
`skills/rics-authenticate-and-read-regulation-scheme.md`). The OpenAPI declares no `operationId`s,
so steps are identified by method and path.

## 1. List the subscriptions

`GET https://api.rics.org/api/Regulation/Subscriptions?regulationSchemeId={id}`

`regulationSchemeId` is an optional query parameter — supply it to scope the result to one scheme.

The response is `RegulationSubscriptions`: an array of `RegulationSubscription` plus
`countOfSubscriptions`. There is no paging; you get the whole set.

Per subscription, read:

- `subscriptionNumber`, `subscriptionProductName`, `numberOfLicences`.
- `startDate` / `endDate` and `status` (`SubscriptionOwnerStatus`).
- `subscriptionUsers[]` — each `SubscriptionUser` carries `contactNo`, `licenceKey`, `status` and
  `approvalStatus`. This is where you find who actually holds a seat.
- `paymentMethod` (`SubscriptionPaymentMethod`) and `sponsorshipCode`.
- `reasonForSuspension`, `reasonForCancellation`, `reasonForEndDateChange` — each is an enum with a
  paired `...Name` label; render the label, key off the enum.
- `regulatedScheme` / `regulatedSchemeName` — the link back to the regulation licence.

## 2. Pull the commercial documents

`GET https://api.rics.org/api/Regulation/PaymentInformation?regulationSchemeId={id}`

Returns `PaymentInformation`: `quotes[]` (`RegulationQuote`), `salesOrders[]`
(`RegulationSalesOrder`), plus `countOfQuotes`, `countOfSalesOrders` and
`countOfSchemeAnnualReturns`.

The chain is **quote → sales order → payment**:

- `RegulationQuote`: `quoteRef`, `totalAmount`, `taxAmount`, `discountAmount`, `isoCurrencyCode`,
  `paymentReceived`, `status` (`QuoteStatus`), `purchaseType`, and `quoteLines[]`.
- `RegulationSalesOrder`: `orderRef`, `totalAmount`, `status` (`SalesOrderStatus`), and
  `quoteNumber`, which is the join back to the quote.

Currency is always explicit (`isoCurrencyCode`, `currencySymbol`) because RICS bills members
worldwide — never assume GBP.

## 3. Resolve a specific payment

`GET https://api.rics.org/api/Payment/{id}`
or
`GET https://api.rics.org/api/Payment/{id}/reference/{reference}`

Both return `PaymentRequestModel`: payer identity (`contactNo`, `firstName`, `lastName`, `email`,
address fields), `currency` / `currencyCode`, `paymentFees` (`Fees[]`), `reference` /
`formattedReference`, `mid` (merchant id), `canPayByIvr`, and **`isTest`**.

`isTest` is the only test-vs-live signal anywhere in this API — RICS publishes no sandbox host, no
test credentials and no test-key prefix. Always check it before treating a payment as real.

## 4. Response codes

- `200` — the document.
- `204` — empty result on the two `Regulation` reads. Not an error.
- `400` — `ProblemDetails` (`application/json`) on the `Regulation` reads.
- `401` — token missing or expired.
- `403` — your credential is not entitled to that scheme or payment.
- `404` — no such payment.

## 5. Do not write from this skill

`POST /api/Payment/update` and `POST /api/OlaMerchantPost` move or record money and **have no
idempotency key** — the API publishes no replay protection, so a retried or duplicated call cannot
be de-duplicated server-side. `POST /api/OlaMerchantPost` additionally declares no description, no
parameters, no request schema and only a `200` response, so its contract cannot be established from
the specification at all. Keep both behind a human decision; see
`agentic-access/rics-agentic-access.yml`.

Payer records here are personal and financial data under UK GDPR. Read the minimum you need.
