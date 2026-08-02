---
name: Authenticate against the DailyPay REST API
description: Obtain, scope, refresh and troubleshoot DailyPay OAuth 2.0 access tokens for both the server-to-server and on-behalf-of-employee paths.
api: openapi/dailypay-rest-openapi-original.yml
operations:
  - getHealth
  - listOrganizations
  - readOrganization
generated: '2026-08-01'
method: generated
source: https://developer.dailypay.com/products/rest/guides/auth
---

# Authenticate against the DailyPay REST API

DailyPay follows OAuth 2.0 (RFC 6749) and OpenID Connect. Every call carries
`Authorization: Bearer <access_token>`. There is no API-key path.

## Pick the flow

| Flow | Scheme | Use when | Scopes |
|---|---|---|---|
| Client credentials | `oauth_client_credentials_token` | Server-to-server; no employee consent needed. Reading connected employer organizations, looking a person up. | `client:admin`, `client:lookup` |
| Authorization code + PKCE | `oauth_user_token` | Acting on an employee's behalf: reading their accounts, requesting a transfer. | `user:read`, `user:read_write` |

Endpoints (authoritative copy at `https://auth.dailypay.com/.well-known/openid-configuration`):

- Authorization: `https://auth.dailypay.com/oauth2/auth`
- Token: `https://auth.dailypay.com/oauth2/token`
- Revocation: `https://auth.dailypay.com/oauth2/revoke`
- UserInfo: `https://auth.dailypay.com/userinfo`
- JWKS: `https://auth.dailypay.com/.well-known/jwks.json`

## Steps

1. **Register first.** There is no self-serve signup. A DailyPay contact registers the application;
   you supply a callback URL, links to your privacy policy and terms of service, optionally a
   consent-screen logo, and optionally a JWKS (or JWKS URL) for signed OIDC requests. DailyPay
   returns `client_id`, `client_secret` (when applicable), the scope list your app may request, and
   the registered `redirect_uri`.

2. **Server-to-server:** POST the client-credentials grant to the token endpoint, requesting only
   the scopes you need (`client:lookup` for person lookup, `client:admin` for broader read).

3. **On behalf of an employee:** run the authorization-code flow **with PKCE** — the scheme
   declares `x-usePkce: true` and the authorization server advertises `S256`. Request `user:read`
   for read-only work and `user:read_write` only when you will write. Add `offline_access` if you
   need a refresh token.

4. **Refresh** with the refresh-token grant rather than re-prompting the user. See the refresh-token
   guide.

5. **Sanity-check connectivity** with `getHealth` (`GET /rest/health`) and your organization wiring
   with `listOrganizations` / `readOrganization` — the client-credentials path's canonical smoke test.

## Scope discipline

`user:read_write` is coarse: one scope covers writes to accounts, jobs, people **and** transfers.
An agent holding it to update a profile also holds the authority to move money. Request `user:read`
by default and escalate to `user:read_write` only for the specific write, for the shortest window
you can manage. See `agentic-access/dailypay-agentic-access.yml`.

## Federation

For embedded experiences, DailyPay supports a trust relationship in which your own IdP asserts the
user over OIDC or SAML, reducing the consent burden. See the trust-relationship guide.

## Elements token handoff

When embedding DailyPay Elements, the Element emits an event when it needs a token. Complete the
authorization-code flow and post back **only the access token** — never the ID token or the refresh
token — to the Element's origin.

## Troubleshooting

- `401 INVALID_TOKEN` — token missing, expired, revoked or malformed. Refresh and retry once.
- `401 UNAUTHORIZED` — no credentials presented.
- `403 FORBIDDEN` — authenticated but not entitled to this resource; usually the wrong scope or the
  wrong token type (a client-credentials token cannot create a transfer).
- `400 INVALID_VERSION_HEADER` — the `DailyPay-API-Version` you sent is unsupported. Current is `3`.
- Confirm you are on the right host: production `https://api.dailypay.com/rest`, UAT
  `https://api.dailypayuat.com/rest`. UAT needs its own credentials.
