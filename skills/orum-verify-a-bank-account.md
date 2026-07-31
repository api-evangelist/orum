---
name: Verify a bank account with Orum Verify
description: Request instant bank account verification (status, ownership, control) and read the result.
api: openapi/orum-openapi-original.yml
operations: [post-oauth-token, post-verify-accounts, get-verify-account, post-verify-account-control, get-verify-accounts]
---

# Verify a bank account with Orum Verify

Instantly verify Account Status, Account Ownership, and Account Control before moving money.

## Auth
Same as Deliver: `post-oauth-token` for a bearer token, send `Orum-Version: v2022-09-21`.
Requires the `read:verify-accounts` / `write:verify-accounts` scopes.

## Steps
1. `post-verify-accounts` (`POST /verify/accounts`) to request verification for a bank account.
2. `get-verify-account` (`GET /verify/accounts/{id}`) to read the verification result and reason codes.
3. (Optional) `post-verify-account-control` (`POST /verify/accounts/{id}/control`) to confirm the end user controls the account.
4. `get-verify-accounts` (`GET /verify/accounts`) to list prior verifications (offset pagination).

## Rules
- Interpret failures via the reason codes reference (see errors/orum-problem-types.yml `related`).
- Subscribe to `verify_account_updated` webhooks for async results (asyncapi/orum-webhooks.yml).
- In sandbox, verification does not hit real banks — outcomes are simulated per the sandbox guides.
