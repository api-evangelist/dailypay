# DailyPay

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

DailyPay is an on-demand pay (earned wage access) platform that lets employees access wages they
have already earned before the scheduled payday. It sells to employers across healthcare, retail,
restaurants, manufacturing, hospitality and the public sector, and integrates with 180+ HCM,
payroll and time-management systems.

- Website — https://www.dailypay.com/
- Developer portal — https://developer.dailypay.com/
- API reference — https://developer.dailypay.com/products/rest/reference
- Status — https://status.dailypay.com/
- GitHub — https://github.com/dailypay

## API

The **DailyPay Rest API** is a json:api implementation covering people, jobs, organizations,
paychecks, accounts (including the `EARNINGS_BALANCE` account carrying available earnings),
transfers, and PCI-scoped debit-card tokenization. It is secured with OAuth 2.0 client-credentials
and authorization-code (PKCE) flows against `auth.dailypay.com`, versioned by the
`DailyPay-API-Version` header, and requires an `Idempotency-Key` on transfer creation.

- Production: `https://api.dailypay.com/rest`
- UAT: `https://api.dailypayuat.com/rest`
- OpenAPI 3.1.0 (18 operations, 95 schemas) — `openapi/dailypay-rest-openapi-original.yml`,
  harvested from https://developer.dailypay.com/_bundle/products/rest/reference/index.yaml

**DailyPay Elements** are hosted iframe components (available earnings, call to action, debit-card
tokenization) that embed on-demand pay with no API integration — see `components/`.

## Artifacts

| Directory | What it holds |
|---|---|
| `openapi/` | The published OpenAPI 3.1.0 bundle, verbatim |
| `overlays/` | OpenAPI Overlay 1.0.0 of our enhancements |
| `authentication/`, `scopes/` | OAuth 2.0 / OIDC profile and the five API scopes |
| `conventions/` | json:api semantics, idempotency, filtering, versioning, tracing |
| `errors/` | 41 problem codes from the json:api `errors[]` envelope, plus 35 transfer/account rejection codes |
| `data-model/` | Entity graph derived from the json:api relationships |
| `lifecycle/` | Header versioning, Statuspage components, health endpoint |
| `sandbox/` | UAT environment and the (absent) test-value surface |
| `conformance/` | json:api, OAuth 2.0, OIDC, PKCE, PCI DSS L1, SOC 2 Type 2, ISO 27001 |
| `security/` | Domain security probe, vulnerability disclosure program, trust center |
| `well-known/` | OIDC discovery + two OAuth authorization-server documents |
| `mcp/` | Two auth-gated MCP endpoints (docs + marketing site) and the REST tool crosswalk |
| `packages/` | Four official Speakeasy-generated SDKs (TypeScript, Go, .NET 8, .NET 9) |
| `components/` | DailyPay Elements |
| `skills/` | Four packaged Agent Skills grounded in real operationIds |
| `agentic-access/` | Recommended `x-agentic-access` contracts for all 18 operations |
| `llms/` | The provider's own `llms.txt`, verbatim |

## Notes

- No webhooks, event surface or AsyncAPI is published.
- No dated changelog or release-notes page is published.
- No first-party CLI.
- No `/.well-known/security.txt` on any host, and no A2A agent card.
- No published rate-limit policy.
