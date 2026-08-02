---
name: Check a worker's available DailyPay earnings
description: Read how much of an employee's earned pay is available to transfer before payday, using the DailyPay REST API earnings-balance account.
api: openapi/dailypay-rest-openapi-original.yml
operations:
  - listAccounts
  - readAccount
  - readPerson
generated: '2026-08-01'
method: generated
source: https://developer.dailypay.com/products/rest/guides/available-earnings
---

# Check a worker's available DailyPay earnings

Available Earnings is the portion of an employee's balance that DailyPay allows them to transfer
before payday. It is never 100% of gross pay — DailyPay applies an advance rate that accounts for
tax and other withholding. The balance lives on an **account** whose `account_type` is
`EARNINGS_BALANCE`.

## Before you start

- Base URL: `https://api.dailypay.com/rest` (UAT: `https://api.dailypayuat.com/rest`).
- Send `Accept: application/vnd.api+json` — this is a json:api API.
- Send `DailyPay-API-Version: 3`. Omitting it silently opts you into the latest version.
- Send `Authorization: Bearer <access_token>`.

## Choose the token

Two paths reach the same balance, and which one you use decides how you scope the query:

- **User token** (`oauth_user_token`, authorization code + PKCE, scope `user:read`) — the employee
  consented; the API already knows who they are.
- **Client-credentials token** (`oauth_client_credentials_token`, scope `client:lookup` or
  `client:admin`) — server-to-server; you must name the person.

## Steps

1. **With a user token — filter by account type.** Call `listAccounts` with
   `filter[account_type]=EARNINGS_BALANCE`.

   ```
   GET /rest/accounts?filter%5Baccount_type%5D=EARNINGS_BALANCE
   ```

2. **With a client-credentials token — filter by person.** Call `listAccounts` with
   `filter[person.id]=<uuid>`. Results are automatically limited to `EARNINGS_BALANCE` accounts on
   this path, so you do not need both filters.

   ```
   GET /rest/accounts?filter%5Bperson.id%5D=<person_uuid>
   ```

3. **Read the balance.** Each returned resource is a json:api object of `type: accounts`. The
   number you want is `attributes.balances.available`, paired with `attributes.balances.currency`.

4. **Convert the amount.** Amounts are integers in minor units. `available: 12000` with
   `currency: USD` is **$120.00** — divide by 100 for USD. Never render the raw integer.

5. **Fetch one account directly** with `readAccount` (`GET /rest/accounts/{account_id}`) when you
   already hold the account UUID and want a fresh read.

6. **Optionally confirm the holder** with `readPerson` (`GET /rest/people/{person_id}`) — it
   returns `state_of_residence` and `disallow_reason`, both of which explain why a balance may
   exist but not be transferable.

## Rules

- `balances.current` may be `null` on an earnings-balance account. Do not treat `null` as zero;
  render only `available`.
- A non-zero available balance does **not** guarantee a transfer will succeed. Eligibility is
  evaluated at transfer time — see the transfer skill and `errors/dailypay-decline-codes.yml`.
- Handle errors from the json:api `errors[]` envelope and branch on `errors[].code`, never on
  `errors[].detail` — the spec states `detail` may change and is not for programmatic use.
- Log `errors[].meta.request_id` and `errors[].meta.trace_id` on every failure; they are the only
  correlation handles DailyPay support can use.
- Relevant failures: `401 INVALID_TOKEN` (refresh the token), `403 FORBIDDEN` (wrong scope or the
  token is not entitled to this person), `400 INVALID_FILTER_FIELD` / `INVALID_FILTER_VALUE`
  (malformed filter).

## No-code alternative

The **Available Earnings Element** renders this same balance in an iframe with no API call. See
`components/dailypay-components.yml`.
