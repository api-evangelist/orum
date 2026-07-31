---
name: Configure and secure Orum webhooks
description: Register a webhook endpoint, set up signature keys, and test event delivery.
api: openapi/orum-openapi-original.yml
operations: [post-oauth-token, post-webhook-secret, get-webhook-secret, post-webhook-configuration, get-webhook-configurations, post-invoke]
---

# Configure and secure Orum webhooks

Receive real-time money-movement events instead of polling.

## Auth
`post-oauth-token` for a bearer token; requires `read:webhook-configurations` / `write:webhook-configurations` and `read:webhook-secret` / `write:webhook-secret` scopes.

## Steps
1. `post-webhook-secret` (`POST /webhooks/secret`) to initialize a public/private key pair; `get-webhook-secret` (`GET /webhooks/secret`) to retrieve it.
2. `post-webhook-configuration` (`POST /webhooks/configurations`) to register your endpoint URL and event subscriptions.
3. `get-webhook-configurations` (`GET /webhooks/configurations`) to confirm registration.
4. `post-invoke` (`POST /webhooks/invoke`) to send a test event to your endpoint.

## Rules
- Verify the signature on every delivery using the key pair — do not put auth in front of the endpoint (keep it publicly reachable). See https://docs.orum.io/api-reference/webhooks/secure-webhooks.
- Event types include `person_verified`, `transfer_updated`, `verify_account_updated`, and more — full list in asyncapi/orum-webhooks.yml.
- Return 2xx quickly and process asynchronously; Orum retries on failure.
