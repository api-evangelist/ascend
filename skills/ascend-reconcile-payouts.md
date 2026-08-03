---
name: Reconcile payouts and invoices
description: List payouts and match them back to their invoices for cash application using the Ascend API V1.
api: openapi/ascend-openapi-original.yml
operations: [listPayouts, listInvoices, getInvoice]
---

# Reconcile payouts and invoices

Ascend API V1 (`https://api.useascend.com`). Bearer-token auth: `Authorization: Bearer <token>`.

## Steps

1. **List payouts** — `listPayouts` (`GET /v1/payouts`, paginate with `page`). Each payout carries an `invoice_id` and status.
2. **List invoices** — `listInvoices` (`GET /v1/invoices`) to build the reconciliation set for the period.
3. **Match** — for each payout, `getInvoice` (`GET /v1/invoices/{id}`) using the payout's `invoice_id` to confirm amounts (`total_amount_cents`) and `status`.

## Rules
- Prefer webhooks over polling: subscribe to `payout.paid`, `payout.failed`, and `invoice.paid` (asyncapi/ascend-webhooks.yml) and verify the `X-Ascend-Signature` HMAC.
- On non-2xx, inspect the `errors[]` array (errors/ascend-problem-types.yml).
