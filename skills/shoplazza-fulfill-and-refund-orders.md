---
name: Process Shoplazza orders (fulfill, cancel, refund)
description: Read orders and run the fulfillment, cancellation, and refund flows on a Shoplazza store via the Admin API.
api: openapi/shoplazza-admin-openapi-original.json
operations: [order-list, order-details, order-details-by-number, cancel-order, close-order, re-open-order, order-refund, partial-order-refund]
---

# Process Shoplazza orders

Read and act on orders in a Shoplazza store.

## Auth
- OAuth 2.0 access token in the `access-token` header.
- Required scopes: `read_order`, `write_order` (and `read_finance`/`write_finance` for refunds touching payment info).

## Steps
1. **Find orders** — `GET /orders` (`order-list`, cursor-paginated) or `GET /orders/number/{number}` (`order-details-by-number`).
2. **Inspect** — `GET /orders/{id}` (`order-details`).
3. **Cancel / close / reopen** — `POST /orders/{id}/cancel` (`cancel-order`), `POST /orders/{id}/close` (`close-order`), `POST /orders/{id}/open` (`re-open-order`).
4. **Refund** — full via `POST /orders/{id}/refund` (`order-refund`) or partial via `POST /orders/{id}/partial_refund` (`partial-order-refund`).

## Rules
- Refunds are irreversible — confirm amount and order state before calling.
- No idempotency keys: guard against duplicate refunds by re-reading order state first.
- Handle `422` validation (e.g. refund exceeds refundable amount) and `429` throttling with backoff.
