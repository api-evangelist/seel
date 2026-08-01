---
generated: '2026-07-21'
method: generated
name: Quote and protect an order
description: Request a Worry-Free Purchase protection quote for a cart, then bind it to an order so a contract is created.
api: openapi/seel-openapi.yml
operations: [createquote, createorder, getorder]
source: >-
  operationIds verified in openapi/seel-openapi.yml (createquote, createorder,
  getorder). Auth and error rules cite authentication/seel-authentication.yml,
  conventions/seel-conventions.yml, and errors/seel-problem-types.yml.
---

# Quote and protect an order

Offer Seel protection at checkout and bind it to the order.

## Auth
- Send `X-Seel-Api-Key: <merchant key>` and `X-Seel-Api-Version: 2.6.0` on every request. See `authentication/seel-authentication.yml`.

## Steps
1. **Create a quote** — `createquote` (`POST /ecommerce/quotes`) with `line_items`, `session_id`, `shipping_address`, `customer`, `type`, `device_category`, `device_platform`, and `is_default_on`. Capture `quote_id`, `price`, and `currency`.
2. **Create the order** — `createorder` (`POST /ecommerce/orders`) with `order_id`, `order_number`, `session_id`, `created_ts`, `line_items`, `shipping_address`, `customer`, and `seel_services: [{ quote_id }]` referencing the quote from step 1.
3. **Confirm coverage** — `getorder` (`GET /ecommerce/orders/{order_id}`). Read `seel_services[].contract_id` to confirm a protection contract was created.

## Errors
- `400` if a required field is missing or does not match the quote; `401` on a bad key; `404` if the quote/order id is unknown. Errors return `{ error, trace_id }` — log `trace_id`. See `errors/seel-problem-types.yml`.

## Notes
- There is no Idempotency-Key header; the client-supplied `order_id` / `order_number` act as the natural de-duplication key on create-order. See `conventions/seel-conventions.yml`.
