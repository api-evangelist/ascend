---
name: Bill and invoice an insured
description: Create an insured, attach it to a program, add billables, and issue then charge an invoice using the Ascend API V1.
api: openapi/ascend-openapi-original.yml
operations: [createInsured, createProgram, createBillable, createInvoice, getInvoice, chargeInvoice]
---

# Bill and invoice an insured

Use the Ascend API V1 (`https://api.useascend.com`, sandbox `https://sandbox.api.useascend.com`).
All requests use bearer-token auth: `Authorization: Bearer <token>`. Request sandbox keys via
developers@useascend.com. Amounts are integer `*_cents` fields; resource ids are UUIDs.

## Steps

1. **Create the insured** — `createInsured` (`POST /v1/insureds`). Capture the returned `id`.
2. **Create the program** — `createProgram` (`POST /v1/programs`) referencing the insured. Capture `program_id`.
3. **Add billables** — `createBillable` (`POST /v1/billables`) for each quote/endorsement line to be billed, referencing the `program_id`.
4. **Create the invoice** — `createInvoice` (`POST /v1/invoices`) to roll the billables into an invoice. Capture the invoice `id`.
5. **Charge the invoice** — `chargeInvoice` to collect payment (or share the `invoice_url` for insured self-service).
6. **Confirm** — `getInvoice` (`GET /v1/invoices/{id}`) and/or subscribe to the `invoice.paid` webhook.

## Rules
- Handle list pagination with the `page` query parameter.
- On non-2xx, read the `errors[]` array in the JSON body (see errors/ascend-problem-types.yml); `422` means validation failed.
- Verify inbound webhooks with the `X-Ascend-Signature` HMAC-SHA256 header (see conventions/ascend-conventions.yml).
