# Manual QA Story 1021 — Validate Domestic Transfer Validation

## Objective

Verify that domestic transfer validation prevents invalid payment requests and permits valid transfers.

## Manual Scenarios

### MQ1 — Valid domestic transfer
Enter a beneficiary account and a positive EUR amount.
Submit the transfer.
Expected: transfer is accepted and confirmation is shown.

### MQ2 — Missing amount
Leave the amount empty.
Expected: transfer cannot be submitted.

### MQ3 — Zero amount
Enter `0.00`.
Expected: transfer cannot be submitted.

### MQ4 — Negative amount
Enter `-10.00`.
Expected: transfer cannot be submitted.

### MQ5 — Missing beneficiary
Leave beneficiary account empty.
Expected: transfer cannot be submitted.

### MQ6 — Missing both mandatory fields
Leave amount and beneficiary account empty.
Expected: transfer cannot be submitted.

## Manual QA Notes

Before automation, clarify whether:
- there is a maximum transfer amount;
- amount precision/decimal-place rules exist;
- a specific beneficiary-account format must be validated;
- exact validation/error messages are contractually required;
- sufficient test accounts exist for successful-transfer automation.
