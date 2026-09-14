# Domestic Transfer API Contract

## Status

Defined for TA Story 119 implementation planning.

## Related Work Items

- Feature 116 — Domestic Money Transfer
- Development Story 117 — DEV: Validate Domestic Transfer Details
- Manual QA Story 118 — MANUAL QA: Validate Domestic Transfer Validation
- TA Story 119 — TA: Automate Domestic Transfer Validation
- Task 122 — Define Domestic Transfer API Contract

## Purpose

Define the API contract required to automate the domestic-transfer
validation scenarios approved in TA Story 119.

This contract supports only the behaviour currently documented by
Feature 116 and Story 117.

It does not introduce undocumented business rules such as:

- maximum transfer amount;
- amount precision rules;
- beneficiary-account format validation;
- foreign-currency behaviour;
- debit-account eligibility rules.

## Endpoint

POST /api/v1/transfers/domestic

## Authentication

Bearer-token authentication is required.

Authentication secrets must be supplied through environment configuration
and must not be committed to source control.

## Content Type

application/json

## Request

```json
{
  "debitAccountId": "TEST-DEBIT-001",
  "beneficiaryAccount": "TEST-BENEFICIARY-001",
  "amount": {
    "value": "25.00",
    "currency": "EUR"
  },
  "message": "Optional payment message"
}