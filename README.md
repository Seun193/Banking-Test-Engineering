# Banking Test Engineering

This repository holds requirement-driven test engineering for retail banking payments. Work is analysed from Azure DevOps source items, constrained by repository governance, and implemented only after human review.

The engineering stack includes:

- Azure DevOps Boards for Features, Development Stories, Manual QA Stories and TA Stories
- Cursor project rules (`.mdc`) for governed AI-assisted QA and test automation
- GitHub for source control and engineering documentation
- Robot Framework, Playwright, Python and API automation
- CI as a quality gate, with retained test evidence

## Core workflow

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

## Governance

AI-assisted analysis does not replace requirement analysis.

Analysis must:

1. read the Feature;
2. read the Development Story and Acceptance Criteria;
3. read the Manual Testing Story;
4. identify gaps and ambiguities;
5. create a traceable Test Automation Story;
6. wait for review before implementing automated tests.

AI-assisted analysis is governed by repository rules and must not invent undocumented business rules, limits, validation behaviour, error messages, or test data.

## Domestic Transfer Validation

**Use case:** Domestic Money Transfer

Domestic transfer validation is an engineering use case for requirement-driven API and UI automation. Source work items define the business goal, development acceptance criteria, and manual QA coverage. Automated coverage is derived from those artefacts after analysis and review.

See:

- `requirements/features/116-domestic-money-transfer.md`
- `requirements/dev-stories/117-transfer-validation.md`
- `requirements/manual-testing/118-transfer-validation-manual-qa.md`
- `docs/test-strategy/domestic-transfer-test-strategy.md`
- `docs/work-items/domestic-transfer-work-item-hierarchy.md`

A Test Automation Story is not included until it has been generated from the source work items and reviewed.
