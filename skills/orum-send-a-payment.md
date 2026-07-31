---
name: Send a payment with Orum Deliver
description: Onboard a customer, link a bank account, and initiate a real-time or ACH transfer via Orum's Deliver API.
api: openapi/orum-openapi-original.yml
operations: [post-oauth-token, post-person, post-external-account, post-eligibility, post-transfer, get-transfer, get-transfer-history]
---

# Send a payment with Orum Deliver

Move money over FedNow, RTP, Same Day ACH, ACH, Wires, or Visa Direct.

## Auth
1. Create API Credentials in Monitor (Developer Tools -> API Authentication).
2. `post-oauth-token` (`POST /oauth/token`) with `client_id`/`client_secret` to get a bearer access token.
3. Send `Authorization: Bearer <token>` and header `Orum-Version: v2022-09-21` on every call.
   Sandbox host is `https://api-sandbox.orum.io`; production is `https://api.orum.io`.

## Steps
1. `post-person` (`POST /deliver/persons`) or `post-business` (`POST /deliver/businesses`) to create the customer party.
2. `post-external-account` (`POST /deliver/external/accounts`) to link the customer's bank account.
3. (Optional) `post-eligibility` (`POST /deliver/eligibility`) to check whether the routing number is RTP/FedNow eligible before choosing a speed.
4. `post-transfer` (`POST /deliver/transfers`) to initiate the transfer between source and destination party; set the `speed` and metadata.
5. `get-transfer` (`GET /deliver/transfers/{id}`) and `get-transfer-history` (`GET /deliver/transfers/{id}/history`) to track status changes.

## Rules
- Pagination on list calls is offset-based: `size` (0-500, default 100) and `index` (default 0); newest first.
- Rate limits use a token bucket; on HTTP 429 (`error_code: too_many_requests`) back off and retry.
- No idempotency-key header exists — do not assume safe automatic retries on `post-transfer`; check status with `get-transfer` before re-initiating.
- Errors return `{ error_code, message, details }` with `application/json`.
- Subscribe to `transfer_updated` webhooks rather than polling; see asyncapi/orum-webhooks.yml.
