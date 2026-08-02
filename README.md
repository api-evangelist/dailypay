# DailyPay

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
