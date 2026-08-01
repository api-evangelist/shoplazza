---
name: Manage Shoplazza products and variants
description: Create, read, update, and delete products and their variants in a Shoplazza store via the Admin API.
api: openapi/shoplazza-admin-openapi-original.json
operations: [product_list, product_show, create_product, update_product, delete_product, create_variant, variant_list, update_variant]
---

# Manage Shoplazza products and variants

Use the Shoplazza Admin API to manage a merchant's catalog.

## Auth
- OAuth 2.0 access token, sent as the `access-token` request header (see `authentication/shoplazza-authentication.yml`).
- Required scopes: `read_product`, `write_product` (see `scopes/shoplazza-scopes.yml`).
- Base URL: `https://{shop}.myshoplaza.com/openapi/{version}` (e.g. `/openapi/2026-01/`).

## Steps
1. **List products** — `GET /products` (`product_list`). Uses cursor pagination: read `data`, `cursor`, and `has_more`; pass `cursor` to page.
2. **Inspect one** — `GET /products/{product_id}` (`product_show`).
3. **Create** — `POST /products` (`create_product`) with title and variants.
4. **Add a variant** — `POST /products/{product_id}/variants` (`create_variant`).
5. **Update** — `PUT /products/{product_id}` (`update_product`) or `PUT /variants/{variant_id}` (`update_variant`).
6. **Delete** — `DELETE /products/{product_id}` (`delete_product`).

## Rules
- No idempotency-key support — do not blindly retry POSTs; check existence first.
- Respect rate limits (leaky bucket: 40 burst / 2 rps app-store, 80 burst / 20 rps store). On `429`, back off.
- Errors return `application/json` with status `400` (bad request), `404` (not found), `422` (validation).
