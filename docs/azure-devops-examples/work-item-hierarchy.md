# Practice Project 1 — Azure DevOps Hierarchy

Recommended Agile-process structure:

Feature — Domestic Money Transfer
├── User Story — DEV: Validate Domestic Transfer Details
├── User Story — MANUAL QA: Validate Domestic Transfer Validation
└── User Story — TA: Automate Domestic Transfer Validation

The three User Stories are siblings under the same Feature.

Recommended tags:
- Development Story: `DEV`
- Manual QA Story: `QA-Manual`
- Test Automation Story: `QA-Automation`
- Training items: `Cursor-Practice`

Recommended links:
- TA Story → Related → Development Story
- TA Story → Related → Manual QA Story

Automation implementation can then be broken into child Tasks under the TA Story, for example:
- Implement API validation tests
- Implement UI validation tests
- Add test data/fixtures
- Add CI execution
- Review/report automation results

Azure DevOps will assign the actual numeric work-item IDs. The 1001/1011/1021 numbers in the repository are sample training references only.
