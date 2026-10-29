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
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

DailyPay is an on-demand pay (earned wage access) platform that lets employees access wages they have already earned before the scheduled payday. The company sells to employers across healthcare, retail, restaurants, manufacturing, hospitality and the public sector, and integrates with 180+ HCM, payroll and time-management systems. Its developer surface — the DailyPay REST API — is a json:api-shaped OAuth 2.0 / OpenID Connect API covering people, jobs, organizations, paychecks, accounts (including the EARNINGS_BALANCE account that carries available earnings), transfers and PCI-scoped debit-card tokenization, complemented by DailyPay Elements, a set of hosted drop-in iframe components for embedding on-demand pay into partner applications.

**Areas:** Company · Payments · Payroll · Human Resources · Earned Wage Access · On-Demand Pay · Financial Services · Fintech · Money Transfer · Benefits

## DailyPay on APIs.io

Everything profiled for DailyPay is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/dailypay/](https://apis.io/providers/dailypay/)** — or go straight to one of the APIs or properties below.

### APIs (8)

- [DailyPay Accounts API](https://apis.io/apis/dailypay/dailypay-accounts-api/) — The _accounts_ endpoint provides comprehensive information about money accounts. You can retrieve account details, including the account's unique ID, a link…
- [DailyPay Card Tokenization API](https://apis.io/apis/dailypay/dailypay-card-tokenization-api/) — Securely tokenize personal cards for use in the accounts API.
- [DailyPay Health API](https://apis.io/apis/dailypay/dailypay-health-api/) — The _health_ endpoint provides a simple health check for the API. **Functionality:** Check the status of the API to ensure it is functioning correctly.
- [DailyPay Jobs API](https://apis.io/apis/dailypay/dailypay-jobs-api/) — The _jobs_ endpoint provides access to comprehensive information about a person's employment. It enables you to retrieve details about individual jobs,…
- [DailyPay Organizations API](https://apis.io/apis/dailypay/dailypay-organizations-api/) — The _organizations_ endpoint provides details about a business entity, such as an employer, or a group of people, such as a division. The response includes…
- [DailyPay Paychecks API](https://apis.io/apis/dailypay/dailypay-paychecks-api/) — The _paychecks_ endpoint provides detailed information about paychecks. You can retrieve individual paycheck details, including the person and job…
- [DailyPay People API](https://apis.io/apis/dailypay/dailypay-people-api/) — The _people_ endpoint allows you to see information related to who owns resources such as jobs and accounts. **Functionality:** Retrieve limited details…
- [DailyPay Transfers API](https://apis.io/apis/dailypay/dailypay-transfers-api/) — The _transfers_ endpoint allows you to initiate and track money movement. You can access transfer details, including the transfer's unique ID, amount,…

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [dailypay-mcp.yml](https://apis.io/servers/dailypay/dailypay-mcp-yml/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Dailypay Agentic Access](https://apis.io/agentic-access/dailypay/dailypay-agentic-access/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Dailypay Rest Overlay](https://apis.io/overlays/dailypay/dailypay-rest-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Dailypay Authentication](https://apis.io/security/dailypay/dailypay-authentication/)
- [Dailypay Domain Security](https://apis.io/security/dailypay/dailypay-domain-security/)
- [Dailypay Trust Center](https://apis.io/security/dailypay/dailypay-trust-center/)
- [Dailypay Vulnerability Disclosure](https://apis.io/security/dailypay/dailypay-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Dailypay Scopes](https://apis.io/scopes/dailypay/dailypay-scopes/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.dailypay.com/)
- [Documentation](https://developer.dailypay.com/products/rest)
- [Getting started](https://developer.dailypay.com/products/rest/guides/auth)
- [API reference](https://developer.dailypay.com/products/rest/reference)
- [Sign up](https://dailypay.app.link/vYyj0iSlyTb)
- [Support](https://www.dailypay.com/contact/)
- [Blog](https://www.dailypay.com/resource-center/blog/)
- [Security](https://www.dailypay.com/security/vulnerability-disclosure-program/)
- [Compliance](https://www.dailypay.com/en-us/legal/compliance/)
- [Terms of service](https://www.dailypay.com/en-us/legal/terms-of-service/)
- [Privacy policy](https://www.dailypay.com/en-us/legal/privacy/)
- [GitHub](https://github.com/dailypay)
- [Status](https://status.dailypay.com/)

## API Evangelist rating

**Kin Score 53.1** — developing · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 75.0 |
| Contract quality | 50.0 |
| Contract governance | 18.2 |
| Operational transparency | 28.9 |
| Developer ergonomics | 73.2 |
| Access clarity | 50.0 |

**Agent readiness 47.8** — agent-ready (gated down from agent-native)

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | verified |
| Auth Clarity | served (partial credit) |
| Idempotency | verified |
| Error Semantics | derived (partial credit) |
| OpenAPI Examples | partial |
| Rate Limit Signal | — |
| Event Surface Described | — |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | served (partial credit) |
| Protected Resource Metadata | verified |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | distinct |

**Regulatory alignment 41.1** — Payments

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/dailypay/](https://apis.io/providers/dailypay/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for DailyPay: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/dailypay/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
