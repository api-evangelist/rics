# RICS (Royal Institution of Chartered Surveyors) (rics)

RICS, the Royal Institution of Chartered Surveyors, is the British royal-chartered professional body founded in London in 1868 that qualifies, regulates and sets standards for surveyors, valuers and built-environment professionals worldwide, with the United Kingdom as its home market. In the property value chain it sits on the professional and valuation side rather than the listings side: it writes the RICS Valuation - Global Standards (the Red Book, incorporating IVS), the RICS Home Survey Standard, RICS Property Measurement / IPMS, ICMS, ILMS and the Rules of Conduct, it regulates roughly 12,000 RICS-regulated firms, and it runs the consumer-facing Find a Surveyor directory at ricsfirms.com and the isurv knowledge platform. Because the United Kingdom has no MLS, there is no RESO here at all - no RESO Web API or Data Dictionary certification, no OData `$metadata`, no Universal Property Identifier - so the "certified but unreachable" pattern does not apply; there is simply no listing-data certification layer in this market. What RICS does publish is genuinely machine-readable: the RICS Data Standard (RDS) 3.3.3 is an MIT-licensed JSON Schema and XSD covering land, property and infrastructure assets and incorporating ICMS, ILMS, IPMS, IVS and IBOS, hosted openly on GitHub at RICS-Data-Standard/RDS and downloadable anonymously. RICS also operates one real production API - the DigitalCommunity API at api.rics.org, whose OpenAPI 3.0.1 contract is served publicly and anonymously from a live Swagger UI - but it is not a public data API: it exposes RICS firm regulation schemes, PII and redress records, subscriptions, payments, member profiles and survey-writer integration, and its own description states that credentials must first be issued by RICS. There is no developer portal, no self-serve signup, and no open dataset from RICS; the UK's open property data layer belongs to HM Land Registry and Ordnance Survey, not to the professional body.

**APIs.json:** [https://raw.githubusercontent.com/api-evangelist/rics/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/rics/refs/heads/main/apis.yml)

## Tags

- Real Estate
- United Kingdom
- Industry Body
- Valuation
- Standards
- Surveying
- Property Measurement
- Regulation
- Construction
- PropTech

## Timestamps

- **Created:** 2026-07-26
- **Modified:** 2026-07-26

## APIs

### RICS DigitalCommunity API

The RICS DigitalCommunity API is a live, RICS-operated REST API served from api.rics.org whose OpenAPI 3.0.1 contract is published anonymously and without credentials at `https://api.rics.org/swagger/v1/swagger.json` (HTTP 200, 67,992 bytes, fetched 2026-07-26) and rendered in a Swagger UI at `https://api.rics.org/`. It carries 16 operations across seven tags — Token, Profile, Regulation, Payment, SurveyWriter, AzureStorage and OlaMerchantPost — and its schemas are unmistakably those of a professional regulator: `RegulationScheme`, `SchemeLicenceType`, `SchemeLicenceStatus`, `PiiInsurer`, `RedressProvider`, `MemberDesignation`, `ProfessionalGrade`, `RegulatedOrganisationType`, `SurveyingService`, `SurveyWriterModel`, `RegulationSubscription` and `RegulationQuote`. This is the machinery behind RICS firm regulation, professional indemnity insurance and redress declarations, subscription billing and survey-writing software integration — it is NOT a property, listings or valuation-data API. Access is not self-serve. The specification's own `info.description` states that "to use this API, you need to have been first been issued a username and password by RICS", which are POSTed as a JSON User object to `/token` to receive a short-lived bearer JWT that must accompany every subsequent request. No application form, pricing page, terms page or developer documentation for this API was found on any RICS property; the closest published route is the RICS Tech Partner Programme. Probed anonymously on 2026-07-26, `GET /api/Profile/1` returned HTTP 401 and `GET /token` returned HTTP 405, confirming the surface is live and enforcing authentication.

- **Human URL:** [https://api.rics.org/](https://api.rics.org/)
- **Base URL:** `https://api.rics.org`

#### Tags

- Regulation
- Membership
- Payments
- Surveying
- Professional Body

#### Properties

- [OpenAPI](openapi/rics-digitalcommunity-api-openapi.json) — harvested verbatim from `https://api.rics.org/swagger/v1/swagger.json` on 2026-07-26 (HTTP 200)
- [API Reference](https://api.rics.org/) — Swagger UI, public and anonymous, "try it out" disabled
- [Partners](https://www.rics.org/get-involved/rics-tech-partner-programme) — RICS Tech Partner Programme

## Common Properties

- [Website](https://www.rics.org/)
- [About](https://www.rics.org/about-rics)
- [Standards and Guidance](https://www.rics.org/profession-standards/rics-standards-and-guidance)
- [Red Book — RICS Valuation Global Standards](https://www.rics.org/profession-standards/rics-standards-and-guidance/sector-standards/valuation-standards/red-book)
- [RICS Property Measurement, 2nd edition (IPMS)](https://www.rics.org/profession-standards/rics-standards-and-guidance/sector-standards/real-estate-standards/rics-property-measurement-2nd-edition)
- [RICS Data Standards](https://www.rics.org/profession-standards/rics-standards-and-guidance/sector-standards/construction-standards/rics-data-standards)
- [JSON Schema — RICS Data Standard 3.3.3](openapi/rics-data-standard-3.3.3-schema.json) (MIT)
- [XML Schema — RICS Data Standard 3.3.3](openapi/rics-data-standard-3.3.3-schema.xsd) (MIT)
- [Data Model — RDS description block](openapi/rics-data-standard-3.3.3-description.json)
- [Example — RDS reference instance](openapi/rics-data-standard-3.3.3-example.json)
- [Example — IPMS](openapi/rics-data-standard-3.3.3-ipms-example.json)
- [Example — ICMS](openapi/rics-data-standard-3.3.3-icms-example.json)
- [Source Code](https://github.com/RICS-Data-Standard/RDS)
- [GitHub Organization](https://github.com/RICS-Data-Standard)
- [OpenID Connect discovery](openapi/rics-azure-ad-b2c-openid-configuration.json)
- [Login](https://services.rics.org/Rics.IntermediaryIdentityService/)
- [Regulation](https://www.rics.org/regulation)
- [Find a Surveyor](https://www.ricsfirms.com/)
- [isurv](https://www.isurv.com/)
- [RICS Tech Partner Programme](https://www.rics.org/get-involved/rics-tech-partner-programme)
- [News and Insights](https://www.rics.org/news-insights)
- [Email — datastandards@rics.org](mailto:datastandards@rics.org)
- [LinkedIn](https://www.linkedin.com/company/rics)

## RESO Posture

**Certified:** No. **Posture:** No RESO reference found anywhere in the RICS estate.

RESO is a North American construct tied to NAR-affiliated MLSs, and the United Kingdom has no MLS to certify against. RICS is neither RESO-certified nor a candidate for certification, holds no listing data, and runs no OData service — so "certified but not reachable" does not apply here; there is no certification layer for property data in this market at all. No `$metadata` document exists anywhere in the estate: `api.rics.org` is an ASP.NET/IIS REST service documented with OpenAPI 3.0.1, not OData. There is no Universal Property Identifier — RICS addresses records by its own regulation-scheme numbers and member/firm identifiers (`/api/Regulation/{schemeNumber}`, `/api/Profile/{id}`), while UK property identity is carried by HM Land Registry title numbers and Ordnance Survey UPRN/TOID.

What RICS standardises instead is the professional layer — the Red Book (incorporating IVS), the Home Survey Standard, IPMS, ICMS, ILMS and the Rules of Conduct — plus one genuinely machine-readable contract: the **RICS Data Standard (RDS) 3.3.3**, an MIT-licensed JSON Schema (draft-04) and XSD published on GitHub at [RICS-Data-Standard/RDS](https://github.com/RICS-Data-Standard/RDS), incorporating ICMS, ILMS, IPMS, IVS and IBOS with buildingSMART IFC and OASIS xAL address formats. Every RDS artifact in `openapi/` was fetched anonymously with HTTP 200 — no account, no EULA click-through, no membership. That is the sharpest contrast with RESO, whose reso.org downloads sit behind a EULA. RDS is a schema and exchange format only: it defines no transport, no endpoints and no authentication, and RICS operates no service that serves or accepts RDS documents over HTTP.

## Access Gate

**Gate:** `partner-only`.

Reading the contract costs nothing — `https://api.rics.org/swagger/v1/swagger.json` returns the full OpenAPI 3.0.1 document to an anonymous GET, and the RDS schemas are MIT-licensed on GitHub. Calling the API is a different matter. The specification states verbatim: *"To use this API, you need to have been first been issued a username and password by RICS. These need to be sent in a JSON User object in the body of a POST request to the /token endpoint in order to receive back a secure bearer token."* No application form, developer terms page, pricing page or signup flow for this API exists on any RICS property. The only published route into a RICS technology relationship is the [RICS Tech Partner Programme](https://www.rics.org/get-involved/rics-tech-partner-programme), a curated, commercially negotiated partner network — hence `partner-only` rather than `application-approval`, because there is no published application to make.

There is no self-serve developer portal. `developer.rics.org`, `developers.rics.org`, `docs.rics.org`, `data.rics.org` and `standards.rics.org` all fail DNS resolution. The nearest thing is the bare Swagger UI at `https://api.rics.org/`, which has `tryItOutEnabled: false`, offers no signup and issues no keys. (`developer.bcis.co.uk` is a real Stoplight developer portal, but BCIS was spun out of RICS in 2022 with LDC backing and RICS retains only a minority stake — it is a separate provider.)

Standards PDFs are linked from `www.rics.org` under `/content/dam/ricsglobal/documents/standards/`, but this could **not** be confirmed anonymously: the host is fronted by Imperva Incapsula and answers every path — including the Red Book PDF — with HTTP 200 and a 212-byte JavaScript challenge instead of the document, under both default and full browser headers. The subscription platform isurv.com carries the same standards behind a paid subscription. Treat the free-PDF claim as unverified rather than as a finding.

## Open Data

**None.** RICS publishes no open, unlicensed, publicly callable dataset. Its market intelligence — the RICS UK Residential Market Survey, Construction Monitor and Commercial Property Monitor — is published as reports and news pages, not as data endpoints. What is open from RICS is *specification*, not data: the MIT-licensed RICS Data Standard schema. The genuinely open UK property data layer belongs to the public sector — HM Land Registry Price Paid Data under the Open Government Licence and Ordnance Survey's open addressing and mapping products — and none of it comes from RICS.

## Auth Model

**DigitalCommunity API:** Bearer JWT via a proprietary username/password token exchange. The specification declares a single security scheme, `Bearer`, as `type: apiKey`, `name: Authorization`, `in: header` — "Please enter into field the word 'Bearer' following by space and JWT" — applied globally. `POST https://api.rics.org/token` takes a JSON User object and returns a short-lived token. Despite issuing JWTs this is **not** OAuth 2.0: no grant type, no `client_id`/`client_secret`, no scopes, no discovery document. Refresh is explicitly pushed onto the client. Anonymous probes on 2026-07-26 returned HTTP 401 from `/api/Profile/1` and HTTP 405 from `GET /token`.

**Member sign-in:** OpenID Connect over Azure AD B2C — tenant `ricsb2clive.onmicrosoft.com` (id `88b1d398-08db-4fc1-af82-65ba1595185c`), custom policy `B2C_1A_RICS_signup_signin`, vanity host `b2clogin.rics.org`, `scopes_supported: ["openid"]`, RS256. The discovery document was fetched anonymously (HTTP 200) and is saved verbatim in `openapi/`. This is the sign-in for RICS members and customers reached through `services.rics.org` — it is **not** a developer authorization surface and issues no API credentials.

## Maintainers

- **Kin Lane** — kin@apievangelist.com
