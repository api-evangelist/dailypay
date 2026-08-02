---
name: Add a payout account (bank or debit card)
description: Tokenize a debit card and attach a DEPOSITORY or CARD account to a DailyPay user so transfers have a destination, then set it as a job's direct-deposit default.
api: openapi/dailypay-rest-openapi-original.yml
operations:
  - createGenericCardToken
  - createAccount
  - listAccounts
  - deleteAccount
  - readJob
  - updateJob
generated: '2026-08-01'
method: generated
source: https://developer.dailypay.com/products/rest/guides/payments
---

# Add a payout account (bank or debit card)

A transfer needs a destination. That destination is an account of type `DEPOSITORY` (a bank
account, subtype `CHECKING` or `SAVINGS`) or `CARD` (a debit card, subtype `DEBIT`).

## Before you start

- `Accept: application/vnd.api+json`, `DailyPay-API-Version: 3`.
- **Token: user token**, scope `user:read_write`, for `createAccount` and `deleteAccount`.
- **Never send raw PAN to `/rest/accounts`.** Card numbers go only to the tokenization endpoint.

## Adding a debit card

1. **Tokenize the card.** `createGenericCardToken` — `POST /cards/generic`. This is a separate,
   PCI-scoped host path; DailyPay documents it as its only PCI-compliant API and states it handles
   card data solely during encryption and tokenization.

   Required fields: `first_name`, `last_name`, `card_number`, `expiration_year` (4-digit),
   `expiration_month` (2-digit), `address_line_one`, `address_city`, `address_state`,
   `address_zip_code`, `address_country`. `cvv` is optional/nullable.

2. **Create the account with the token.** `createAccount` — `POST /rest/accounts` — using the token
   returned in step 1, with `account_type: CARD` and `subtype: DEBIT`.

3. Discard the raw card data. Only the token should ever reach your storage.

**No-code alternative:** the **Debit Card Tokenization Element** performs steps 1 and 2's data
collection inside a DailyPay-hosted iframe, so raw card data never touches your application at all.
See `components/dailypay-components.yml`.

## Adding a bank account

Call `createAccount` with `account_type: DEPOSITORY` and `subtype: CHECKING` or `SAVINGS`.

## Setting it as the direct-deposit default

1. Read the employee's job with `readJob` (`GET /rest/jobs/{job_id}`) and check
   `attributes.activation_status`.
2. Call `updateJob` (`PATCH /rest/jobs/{job_id}`) to set
   `relationships.direct_deposit_default_depository` (a `DEPOSITORY` account) or
   `relationships.direct_deposit_default_card` (a `CARD` account, used as the fallback when the
   depository deposit fails).

The job must already be `ACTIVATED` — otherwise `updateJob` returns `400 ACTIVE_JOB_REQUIRED`.

## Removing an account

`deleteAccount` (`DELETE /rest/accounts/{account_id}`) removes a previously added `DEPOSITORY` or
`CARD` account. **`EARNINGS_BALANCE` accounts cannot be deleted** — do not attempt it.

## Reading a rejection

`createAccount` returns `400` with these codes in `errors[].code` (full list in
`errors/dailypay-decline-codes.yml`):

| Code | What to do |
|---|---|
| `ACCOUNT_TYPE_INVALID` | Type must be `DEPOSITORY` or `CARD` |
| `ACCOUNT_SUBTYPE_INVALID` | Subtype must be `CHECKING`, `SAVINGS` or `DEBIT` |
| `ACCOUNT_TYPE_SUBTYPE_MISMATCH` | Subtype is not valid for that type |
| `DUPLICATE_ACCOUNT` | Account already exists — look it up with `listAccounts` instead of retrying |
| `INVALID_CARD_TOKEN` | Token expired or malformed — re-tokenize |
| `INVALID_DEBIT_CARD` | Card is not acceptable |
| `DEBIT_CARD_CREATION_BLOCKED` | Blocked BIN — the user must use a different card |
| `BANK_ACCOUNT_CREATION_BLOCKED` | Blocked routing/account combination |
| `MISSING_REQUIRED_FIELD` / `INVALID_FIELDS` | Fix the payload before retrying |

## Rules

- Branch on `errors[].code`, never on `errors[].detail`. Treat `code` as an open enum.
- Do not log, cache or persist `card_number` or `cvv` anywhere. The token is the only artefact you
  keep.
- `createAccount` carries no idempotency key — guard against double submission on your side, and on
  a timeout call `listAccounts` to check whether the account landed before retrying.
- Log `meta.request_id` and `meta.trace_id` on failures.
