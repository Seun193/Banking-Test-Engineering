# AI Test Automation Lab

A hands-on portfolio and training project for practising AI-assisted Test Automation using:

- Azure DevOps Boards for Features, Development Stories, Manual QA Stories and TA Stories
- Cursor project rules (`.mdc`) for governed AI-assisted QA/TA work
- GitHub for source control and portfolio documentation
- Robot Framework, Playwright, Python and API automation as the lab grows
- CI/CD later in the project

## Core workflow

```text
Business Requirement
        ↓
Azure DevOps Feature
        ↓
Development Story
        ↓
Manual QA Story
        ↓
Cursor + MDC Requirement Analysis
        ↓
Test Automation Story
        ↓
Human TA Review / Approval
        ↓
Automation Implementation
        ↓
Execution + CI
        ↓
Evidence / Results
```

## Important principle

AI does not replace requirement analysis.

Cursor must:
1. read the Feature;
2. read the Development Story and Acceptance Criteria;
3. read the Manual Testing Story;
4. identify gaps and ambiguities;
5. create a traceable Test Automation Story;
6. wait for review before implementing automated tests.

Cursor must not invent missing business rules, limits, units, error messages or test data.

## Practice Project 1

**Feature:** Domestic Money Transfer

This first exercise deliberately contains enough detail to begin meaningful TA analysis while leaving room to practise requirement-gap detection.

See:
- `requirements/features/1001-domestic-money-transfer.md`
- `requirements/dev-stories/1011-transfer-validation.md`
- `requirements/manual-testing/1021-transfer-validation-manual-qa.md`

The TA Story is intentionally **not included**. Cursor should generate it from the source work items.
