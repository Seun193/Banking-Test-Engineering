# Domestic Transfer — Azure DevOps Work Item Hierarchy

Agile-process structure:

```text
Feature 116 — Domestic Money Transfer
├── User Story 117 — DEV: Validate Domestic Transfer Details
├── User Story 118 — MANUAL QA: Validate Domestic Transfer Validation
└── User Story — TA: Automate Domestic Transfer Validation
```

The three User Stories are siblings under the same Feature.

Recommended tags:

- Development Story: `DEV`
- Manual QA Story: `QA-Manual`
- Test Automation Story: `QA-Automation`

Recommended links:

- TA Story → Related → Development Story
- TA Story → Related → Manual QA Story

Automation implementation can then be broken into child Tasks under the TA Story, for example:

- Implement API validation tests
- Implement UI validation tests
- Add test data/fixtures
- Add CI execution
- Review/report automation results
