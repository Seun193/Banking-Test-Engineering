# Development Story 1011 — Validate Domestic Transfer Details

## User Story

As an authenticated retail banking customer,
I want the transfer form to validate my payment details,
so that invalid domestic transfers are not submitted.

## Acceptance Criteria

### AC1 — Amount required
Given the customer is creating a domestic transfer,
when no transfer amount is provided,
then the transfer cannot be submitted.

### AC2 — Amount must be positive
Given the customer is creating a domestic transfer,
when the transfer amount is zero or below,
then the transfer cannot be submitted.

### AC3 — Currency
Domestic transfer amounts are processed in EUR.

### AC4 — Beneficiary account required
Given the customer is creating a domestic transfer,
when the beneficiary account number is empty,
then the transfer cannot be submitted.

### AC5 — Successful submission
Given the customer is authenticated,
and all mandatory transfer data is valid,
when the customer submits the transfer,
then the transfer is accepted
and a confirmation is returned.

## Technical Notes

- Transfer validation is exposed through the application transfer API.
- The UI uses the same backend validation.
- Automated API and UI coverage may both be considered.
