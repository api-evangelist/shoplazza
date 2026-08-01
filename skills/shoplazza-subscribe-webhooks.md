---
name: Subscribe to Shoplazza store events with webhooks
description: Register, list, update, and delete webhook subscriptions so an app receives Shoplazza store events.
api: openapi/shoplazza-admin-openapi-original.json
operations: [create-webhook, webhook-list, webhook-count, webhook-details, update-webhook, delete-webhook]
---

# Subscribe to Shoplazza store events

Register webhook subscriptions so Shoplazza POSTs store events to your HTTPS endpoint.

## Auth
- OAuth 2.0 access token in the `access-token` header.
- Verify inbound webhook payloads with the HMAC signature (same algorithm as OAuth callbacks).

## Steps
1. **Subscribe** — `POST /webhooks` (`create-webhook`). Body for 2022-01 is flat `{topic, address}`; from 2025-06 onward wrap it: `{"webhook": {"topic": "products/create", "address": "https://your-app.example.com/hook"}}`.
2. **List / count** — `GET /webhooks` (`webhook-list`), `GET /webhooks/count` (`webhook-count`).
3. **Inspect** — `GET /webhooks/{id}` (`webhook-details`).
4. **Update / delete** — `PUT /webhooks/{id}` (`update-webhook`), `DELETE /webhooks/{id}` (`delete-webhook`).

## Rules
- `address` must be HTTPS and publicly reachable.
- Topics follow `resource/action` (e.g. `products/create`, `orders/create`, `app/uninstalled`); see the Webhook Events reference for the full list.
- The subscription body shape differs by API version — match it to your `/openapi/YYYY-MM/` prefix.
