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
```

## Validation Behaviour

### Missing amount

A request with no transfer amount must be rejected.

Expected HTTP status:

```text
400
```

Expected response:

```json
{
  "status": "REJECTED",
  "error": {
    "code": "VALIDATION_FAILED",
    "fields": ["amount"]
  }
}
```

### Zero amount

An amount equal to zero must be rejected.

Expected HTTP status:

```text
400
```

Expected response:

```json
{
  "status": "REJECTED",
  "error": {
    "code": "VALIDATION_FAILED",
    "fields": ["amount"]
  }
}
```

### Negative amount

An amount below zero must be rejected.

Expected HTTP status:

```text
400
```

Expected response:

```json
{
  "status": "REJECTED",
  "error": {
    "code": "VALIDATION_FAILED",
    "fields": ["amount"]
  }
}
```

### Missing beneficiary

A request with no beneficiary account must be rejected.

Expected HTTP status:

```text
400
```

Expected response:

```json
{
  "status": "REJECTED",
  "error": {
    "code": "VALIDATION_FAILED",
    "fields": ["beneficiaryAccount"]
  }
}
```

### Missing amount and beneficiary

A request with both required values missing must be rejected.

Expected HTTP status:

```text
400
```

Expected response:

```json
{
  "status": "REJECTED",
  "error": {
    "code": "VALIDATION_FAILED",
    "fields": [
      "amount",
      "beneficiaryAccount"
    ]
  }
}
```

## Successful Transfer

A valid domestic transfer request is accepted.

Expected HTTP status:

```text
201
```

Expected response structure:

```json
{
  "status": "ACCEPTED",
  "transferId": "TRN-TEST-0001",
  "confirmation": {
    "reference": "CONF-TEST-0001"
  }
}
```

The generated `transferId` and confirmation reference are dynamic values and must not be asserted as fixed identifiers.

## Automation Assertions

Automated tests may assert:

- HTTP status;
- response `status`;
- relevant validation field names;
- presence of `transferId` on successful requests;
- presence of a confirmation reference on successful requests.

Automated tests must not assert undocumented human-readable error text.

## Out of Scope

The following remain outside the current contract:

- maximum transfer amount;
- amount decimal-place or precision rules;
- malformed beneficiary-account validation;
- unauthenticated behaviour;
- foreign-currency handling;
- debit-account eligibility logic;
- optional payment-message validation rules.

These behaviours require additional documented requirements before automation scenarios are introduced.