---
name: Request an on-demand pay transfer
description: Move earned wages from a DailyPay EARNINGS_BALANCE account to an employee's bank or card account, idempotently, and interpret the rejection codes.
api: openapi/dailypay-rest-openapi-original.yml
operations:
  - listAccounts
  - createTransfer
  - readTransfer
  - listTransfers
generated: '2026-08-01'
method: generated
source: https://developer.dailypay.com/products/rest/reference/transfers
---

# Request an on-demand pay transfer

A transfer moves value from the employee's `EARNINGS_BALANCE` account (the origin) to a
`DEPOSITORY` or `CARD` account (the destination). **This moves real money.** Treat it as a
physical-consequence operation: confirm intent with the user before calling it, and never retry it
blindly.

## Before you start

- Base URL: `https://api.dailypay.com/rest` (UAT: `https://api.dailypayuat.com/rest`).
- `Accept: application/vnd.api+json`, `DailyPay-API-Version: 3`.
- **Token: user token only.** `createTransfer` requires `oauth_user_token` with scope
  `user:read_write`. A client-credentials token cannot initiate a transfer.

## Steps

1. **Find the origin.** Call `listAccounts` with `filter[account_type]=EARNINGS_BALANCE` and read
   `attributes.balances.available`. That integer, in minor units, is the ceiling for this transfer.

2. **Find the destination.** Call `listAccounts` again filtering on
   `filter[account_type]=DEPOSITORY` or `filter[account_type]=CARD`. If none exists, add one first
   — see the add-a-payout-account skill.

3. **Mint an idempotency key.** Generate a fresh UUID v4 and send it as the **required**
   `Idempotency-Key` header. Persist it alongside your own transfer intent record *before* you
   issue the request, so a crash mid-flight can safely replay the exact same key and payload.

4. **Create the transfer.** `createTransfer` — `POST /rest/transfers`. The json:api request body
   carries the amount and currency in `data.attributes`, and the `origin` and `destination`
   relationships in `data.relationships`.

5. **Read the result.** The 200 response is a `type: transfers` resource. Poll `readTransfer`
   (`GET /rest/transfers/{transfer_id}`) for status changes, or list recent activity with
   `listTransfers` and `filter[submitted_at__gt]=<timestamp>`.

6. **Inspect funding (optional).** `readTransfer` and `listTransfers` accept
   `include=estimated_funding_sources,final_funding_sources` to show which paychecks are being
   drawn against. Under continuous pay access a single transfer can span two pay periods, so the
   estimate can differ from the final allocation.

## Idempotency rules

- The key is **required**, and must be a UUID.
- Same key + **same payload** → the identical response is replayed. Safe.
- Same key + **different payload** → `400 INVALID_IDEMPOTENCY_KEY`. Never reuse a key for a
  different amount or destination.
- Same key while a first request is still in flight → `409 IDEMPOTENCY_KEY_LOCKED`. The spec states
  this is **safe to retry** — back off and retry with the same key.
- On any network timeout, retry with the **same** key. Do not mint a new one; that is how you
  double-pay someone.

## Reading a rejection

Rejections come back as `400` in the json:api `errors[]` envelope. Branch on `errors[].code`. The
full catalogue is in `errors/dailypay-decline-codes.yml`; the ones worth handling explicitly:

| Code | What it means | What to do |
|---|---|---|
| `EARNINGS_BALANCE_EXCEEDED` | Asked for more than is available | Re-read the balance and re-prompt |
| `MINIMUM_AMOUNT_SUBCEEDED` | Below the provider minimum (generally $5) | Raise the amount |
| `MAXIMUM_AMOUNT_EXCEEDED` | Above the per-transfer cap | Split or lower the amount |
| `TRANSFER_AMOUNT_LIMIT_EXCEEDED` / `TRANSFER_LIMIT_EXCEEDED` | 24-hour value or count limit hit | Tell the user to wait; do not retry |
| `DUPLICATE_TRANSFER` | Same amount and destination seen recently | Stop. Confirm with the user before retrying |
| `DISCLOSURE_REQUIRED` | Wage Disclosure not signed | Route the user to sign it |
| `MISSING_STATE_OF_RESIDENCE` | `state_of_residence` unset on the person | Set it via `updatePerson`, then retry |
| `MISSING_DEFAULT_DEPOSITORY_ACCOUNT` | No default bank account on the related jobs | Set one via `updateJob` |
| `INVALID_DIRECT_DEPOSIT_STATUS` | Job's `direct_deposit_status` is not `SETUP_COMPLETE` | Not fixable in-session |
| `OVERPAYMENT_RESOLVING` / `OVERPAYMENT_RESOLUTION_REQUIRED` | Prior overpayment outstanding | Stop; human resolution needed |
| `TRANSFERS_DISABLED` / `INELIGIBLE_ORIGIN` | Origin cannot transfer right now | Stop |

Treat `code` as an **open enum** — DailyPay states new codes may be added, so always have a default
branch that surfaces the code verbatim rather than crashing.

## Rules

- Never retry a `400` unchanged. Only `409 IDEMPOTENCY_KEY_LOCKED` and transport failures are
  retryable.
- Amounts are integers in minor units with an ISO 4217 currency; `$120.00 USD` is `12000` + `USD`.
- Do not surface `errors[].detail` as a stable string — surface the `code`.
- Log `meta.request_id` and `meta.trace_id` for every failure.
