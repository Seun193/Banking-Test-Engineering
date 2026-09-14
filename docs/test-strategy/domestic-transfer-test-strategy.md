# Domestic Transfer Test Strategy

## Purpose

Define how domestic transfer validation is analysed, reviewed, and automated from Azure DevOps source work items. The approach is requirement-driven: coverage is derived from documented behaviour, not from inferred product rules.

## Source work items

- Feature 116 — Domestic Money Transfer
- Development Story 117 — Validate Domestic Transfer Details
- Manual QA Story 118 — Validate Domestic Transfer Validation
- Governance rules in `.cursor/rules/`

## Delivery workflow

```text
Business Requirement
        →
Azure DevOps Feature
        →
Development Story
        →
Manual QA Analysis
        →
Test Automation Analysis
        →
TA Story
        →
Human Review
        →
API / UI Automation
        →
CI Quality Gate
        →
Test Evidence
```

## Analysis approach

Test automation analysis reads Feature 116, Development Story 117, and Manual QA Story 118, then applies the Test Automation Story Generator rule to produce a reviewable TA Story. Automation code is not implemented until that story has been reviewed.

The analysis must:

- read all three source work items;
- map proposed scenarios to acceptance criteria;
- identify unresolved maximum-amount behaviour;
- identify unresolved amount precision;
- identify beneficiary-format ambiguity;
- avoid inventing exact error messages;
- distinguish API and UI automation;
- mark blocked scenarios as `BLOCKED` or `NEEDS CLARIFICATION`;
- withhold implementation until after review.

## Open questions affecting automation

These items are documented as unresolved in Manual QA notes and must not be invented during analysis:

- whether a maximum transfer amount applies;
- amount precision and decimal-place rules;
- whether a specific beneficiary-account format must be validated;
- whether exact validation or error messages are contractually required;
- whether sufficient test accounts exist for successful-transfer automation.

## Governance

AI-assisted analysis is governed by repository rules and must not invent undocumented business rules, limits, validation behaviour, error messages, or test data.

The TA Story is a review artefact. API and UI automation follow human approval, then run through the CI quality gate with retained test evidence.
