---
name: Set up premium financing for a program
description: Look up a program, estimate a loan payoff, and inspect the resulting financing loan using the Ascend API V1.
api: openapi/ascend-openapi-original.yml
operations: [listPrograms, getProgram, createLoanPayoffEstimate, getLoan, listLoans]
---

# Set up premium financing for a program

Ascend API V1 (`https://api.useascend.com`). Bearer-token auth: `Authorization: Bearer <token>`.
Monetary values are integer `*_cents`; ids are UUIDs.

## Steps

1. **Find the program** — `listPrograms` (`GET /v1/programs`, paginate with `page`) then `getProgram` (`GET /v1/programs/{id}`) to load its detail.
2. **Estimate a loan payoff** — `createLoanPayoffEstimate` (`POST /v1/programs/{id}/loan_payoff_estimate`) to compute the payoff for the financed premium.
3. **Inspect the loan** — `listLoans` (`GET /v1/loans`) to find the associated loan, then `getLoan` (`GET /v1/loans/{id}`) for installment and balance detail.

## Rules
- Read the `errors[]` array on non-2xx responses (errors/ascend-problem-types.yml).
- Loans and installment plans are downstream of programs; always resolve the `program_id` first.
- Subscribe to `payout.*` webhooks to track disbursement (asyncapi/ascend-webhooks.yml).
