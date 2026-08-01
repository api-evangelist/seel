---
generated: '2026-07-21'
method: generated
name: Track a claim to resolution
description: List claims against protection contracts and follow one to resolution, subscribing to claim.* webhooks.
api: openapi/seel-openapi.yml
operations: [listclaims, getclaim, getcontract]
source: >-
  operationIds verified in openapi/seel-openapi.yml (listclaims, getclaim,
  getcontract). Webhook events cite asyncapi/seel-webhooks-asyncapi.yml.
---

# Track a claim to resolution

Monitor customer claims on Seel protection contracts and follow one to resolution.

## Auth
- Send `X-Seel-Api-Key` and `X-Seel-Api-Version` on every request. See `authentication/seel-authentication.yml`.

## Steps
1. **List claims** — `listclaims` (`GET /ecommerce/claims`) to enumerate open claims. Capture each `claim_id` and its `contract_id`.
2. **Inspect a claim** — `getclaim` (`GET /ecommerce/claims/{claim_id}`) to read status and payout details.
3. **Cross-reference the contract** — `getcontract` (`GET /ecommerce/contracts/{contract_id}`) to confirm coverage and premium.

## Webhooks (preferred over polling)
- Subscribe to `claim.created`, `claim.accepted`, `claim.rejected`, `claim.resolved`, `claim.reopened`, and `claim.closed`. Each is a Notification `{ id, created_ts, type, data }`. Register via `merchant@seel.com`. See `asyncapi/seel-webhooks-asyncapi.yml`.

## Errors
- `404` if a claim or contract id is unknown; `401` on a bad key. Errors return `{ error, trace_id }`. See `errors/seel-problem-types.yml`.
