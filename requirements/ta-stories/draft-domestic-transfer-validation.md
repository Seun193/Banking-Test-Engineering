# TA Story: DRAFT — Automate Domestic Transfer Validation

**Status:** DRAFT — for Test Automation Engineer review. Not approved for implementation.

## Objective

Produce a reviewable automation scope for domestic transfer validation, traced from Feature 116 and Development Story 117 acceptance criteria through Manual QA Story 118. Prove that invalid required details cannot complete a transfer, and that a valid authenticated transfer is accepted and confirmed — without inventing undocumented rules, limits, formats, error text, or test data.

## Source Work Items

| Type | ID | Title |
| --- | --- | --- |
| Feature | 116 | Domestic Money Transfer |
| Development User Story | 117 | DEV: Validate Domestic Transfer Details |
| Manual QA User Story | 118 | MANUAL QA: Validate Domestic Transfer Validation |
| Test Automation Story | — | Not yet created as an Azure DevOps work item |

## Requirement Traceability

| Source | Requirement / criterion | Manual QA | Proposed automation | Status |
| --- | --- | --- | --- | --- |
| Feature 116 | Authenticated retail customers can create a domestic money transfer | Implied by MQ1 (authenticated context not stated) | TA-P01 (success). Unauthenticated access is a documented risk only (see Future Coverage) | TA-P01: scenario design ready; automation implementation `BLOCKED` (test data + API/UI contract) |
| Feature 116 | Customer selects an eligible debit account | Not covered | — | Requirement gap. No AC in Story 117; eligibility rules not documented. Do not invent scenarios or eligibility rules |
| Feature 116 | Customer provides a beneficiary account number | MQ1, MQ5, MQ6 | TA-P01, TA-V02, TA-V03 | Presence/emptiness only. Format rules not documented |
| Feature 116 | Customer provides a transfer amount in EUR | MQ1–MQ4, MQ6 | TA-P01, TA-V01, TA-B01, TA-B02. Currency representation: Future Coverage (TA-V04) | Amount required / zero / below-zero: scenario design READY. AC3 `NEEDS CLARIFICATION` |
| Feature 116 | Optional payment message | Not covered | — | Requirement gap / Future Coverage (TA-P02). Not in current implementation scope |
| Feature 116 | Transfer must not be completed when required information is invalid | MQ2–MQ6 | TA-V01–TA-V03, TA-B01, TA-B02 | Scenario design READY. Automation implementation `BLOCKED` pending API contract / observable response contract |
| Feature 116 | Successful transfers return a confirmation | MQ1 | TA-P01 (API), TA-U01 (UI confirmation shown) | Automation implementation `BLOCKED` until controlled test data exists |
| Feature 116 | Out of scope: international, scheduled recurring, foreign-currency | Not covered | — | Out of scope. Do not add scenarios that invent rejection rules for those products |
| Story 117 AC1 | Amount required — empty amount cannot be submitted | MQ2, MQ6 | TA-V01, TA-V03 | Scenario design: READY. Automation implementation: `BLOCKED` pending API contract |
| Story 117 AC2 | Amount zero or below cannot be submitted | MQ3, MQ4 | TA-B01, TA-B02 | Scenario design: READY. Automation implementation: `BLOCKED` pending API contract. Zero is the only documented numeric boundary |
| Story 117 AC3 | Amounts processed in EUR | MQ1 uses “positive EUR amount”; no currency-negative case | TA-V04 | Future Coverage. `NEEDS CLARIFICATION`. No Given/When/Then; no invalid-currency behaviour |
| Story 117 AC4 | Empty beneficiary cannot be submitted | MQ5, MQ6 | TA-V02, TA-V03 | Scenario design: READY. Automation implementation: `BLOCKED` pending API contract |
| Story 117 AC5 | Authenticated customer, valid mandatory data → accepted + confirmation | MQ1 | TA-P01, TA-U01 | Automation implementation `BLOCKED` until controlled successful-transfer test data exists |
| Story 118 MQ6 | Both mandatory fields empty | Combination of AC1 + AC4 | TA-V03 | Scenario design: READY. Supported by documented required-field behaviour; not a separate AC |

### Development vs Manual QA comparison

**Covered by both Story 117 and Story 118**

- Missing amount (AC1 / MQ2)
- Zero amount (AC2 / MQ3)
- Negative amount (AC2 / MQ4)
- Missing beneficiary (AC4 / MQ5)
- Successful submission with confirmation (AC5 / MQ1)

**In Story 117 / Feature 116 but missing from Manual QA**

- AC3 currency as an explicit check (EUR processing vs how currency is supplied)
- Feature 116 eligible debit-account selection
- Feature 116 optional payment message
- Unauthenticated or otherwise non-retail access (Feature 116 and AC5 require authentication; no negative MQ)

**Manual scenarios and requirement support**

- MQ1–MQ5 map directly to AC5, AC1, AC2, AC2, AC4
- MQ6 is not a separate AC; it is a valid combination of AC1 and AC4 and does not contradict documented behaviour
- No Manual QA scenario is unsupported by the source requirements

**Inconsistencies**

- Feature 116 requires an eligible debit account; Story 117 ACs and Story 118 scenarios do not mention debit-account selection
- AC3 is a statement (“processed in EUR”) rather than observable submit/reject behaviour
- Feature 116 “must not be completed”, Story 117 “cannot be submitted”, and Story 118 “cannot be submitted” / “confirmation is shown” do not specify the API response contract used to observe rejection or confirmation
- MQ1 does not state authentication or debit-account preconditions that Feature 116 and AC5 require

**Acceptance-criteria gaps (do not invent values)**

- Maximum transfer amount: not documented
- Amount precision / decimal-place rules: not documented (`0.00` and `-10.00` appear only as MQ examples of zero and below-zero)
- Smallest accepted positive amount: not documented (“positive” is used; no minimum increment)
- Beneficiary-account format: not documented
- Confirmation structure (fields, UI confirmation, API body): not documented
- Optional payment-message rules: not documented beyond optionality on Feature 116
- Debit-account eligibility: not documented (requirement gap; no eligibility rules invented)
- Transfer API endpoint, payload and observable response contract: not documented
- Duplicate submit, expired session, and dependent-service failure: not documented — omitted from scope
- Exact validation/error-message wording: not documented and **non-blocking**. Tests verify documented rejection (transfer cannot be submitted / is not completed). Exact UI/API message text is asserted only if it later becomes a contractual requirement

## Automation Scope

**Current implementation scope (after review; automation still blocked as noted)**

- **API (primary automation layer):** Story 117 states that transfer validation is exposed through the application transfer API and that the UI uses the same backend validation. Detailed validation therefore belongs at API level:
  - missing amount
  - zero amount
  - negative amount
  - missing beneficiary
  - both mandatory fields empty
- **API success path:** valid submit accepted and confirmation returned — when controlled test data exists
- **UI (thin):** successful confirmation shown (MQ1) — when controlled test data exists. Do not duplicate every backend validation scenario in the UI

**Out of current implementation scope**

- International, scheduled recurring, and foreign-currency products (Feature 116)
- Asserting exact error-message wording (non-blocking unless it becomes a contractual requirement)
- Maximum-amount, precision, and beneficiary-format cases until those rules are documented
- Invented debit-account eligibility matrices (eligibility remains a requirement gap)
- Optional payment message (TA-P02) — Future Coverage
- Unauthenticated transfer (TA-N07) — Future Coverage
- Currency processing as an executable AC3 test (TA-V04) — Future Coverage
- Database assertions (no data model documented)
- Implementation of automated tests (this artefact is analysis only)

## Scenarios to Automate

### Positive

#### TA-P01 — Valid domestic transfer is accepted and confirmed

- **Source:** Feature 116 (success + confirmation); Story 117 AC5; Story 118 MQ1
- **Test intent:** Authenticated customer submits mandatory valid transfer data; transfer is accepted and a confirmation is returned
- **Expected result:** Transfer is accepted; a confirmation is returned at API level
- **Automation level:** API (primary). UI confirmation shown is TA-U01 only
- **Scenario design:** READY (AC5 / MQ1)
- **Automation implementation:** `BLOCKED` — sufficient controlled test accounts / valid beneficiary and debit-account data are not documented; API contract and confirmation observation are unspecified; “valid” amount and beneficiary cannot be constructed without inventing business-valid data

### Negative

#### TA-V01 — Missing amount cannot be submitted

- **Source:** Story 117 AC1; Story 118 MQ2; Feature 116 (invalid required information)
- **Test intent:** Create a domestic transfer with no transfer amount
- **Expected result:** Transfer cannot be submitted / is not completed. Do not invent or assume exact error-message text
- **Automation level:** API (primary automation layer)
- **Scenario design:** READY
- **Automation implementation:** `BLOCKED` pending API contract (endpoint, payload) and observable response contract that proves non-completion

#### TA-V02 — Missing beneficiary cannot be submitted

- **Source:** Story 117 AC4; Story 118 MQ5
- **Test intent:** Beneficiary account number empty
- **Expected result:** Transfer cannot be submitted / is not completed. Do not invent or assume exact error-message text
- **Automation level:** API (primary automation layer)
- **Scenario design:** READY
- **Automation implementation:** `BLOCKED` pending API contract / observable response contract. Do not add invalid-format variants

#### TA-V03 — Missing amount and beneficiary cannot be submitted

- **Source:** Story 118 MQ6; Story 117 AC1 + AC4
- **Test intent:** Both mandatory fields empty
- **Expected result:** Transfer cannot be submitted / is not completed. Do not invent or assume exact error-message text
- **Automation level:** API (primary automation layer)
- **Scenario design:** READY
- **Automation implementation:** `BLOCKED` pending API contract / observable response contract. Combined check; does not replace TA-V01 or TA-V02

### Boundary

Documented numeric boundary is **zero**. “Below zero” is documented as invalid. No maximum and no precision rule are documented; those boundaries are not proposed as executable tests.

#### TA-B01 — Zero amount cannot be submitted

- **Source:** Story 117 AC2; Story 118 MQ3
- **Test intent:** Amount `0.00` as used in MQ3 (representation of zero in that scenario, not a precision specification)
- **Expected result:** Transfer cannot be submitted / is not completed. Do not invent or assume exact error-message text
- **Automation level:** API (primary automation layer)
- **Scenario design:** READY
- **Automation implementation:** `BLOCKED` pending API contract / observable response contract, including how zero is encoded on the request. Do not treat MQ3 as a decimal-place business rule

#### TA-B02 — Negative amount cannot be submitted

- **Source:** Story 117 AC2; Story 118 MQ4
- **Test intent:** Amount below zero; MQ4 uses `-10.00` as an example of below-zero, not as a unique business limit
- **Expected result:** Transfer cannot be submitted / is not completed. Do not invent or assume exact error-message text
- **Automation level:** API (primary automation layer)
- **Scenario design:** READY
- **Automation implementation:** `BLOCKED` pending API contract / observable response contract, including how a negative amount is encoded. Do not add further negative values as extra business cases

#### Not proposed (undocumented)

- Maximum amount, just-below/just-above a maximum — omitted until a maximum is documented
- Smallest positive increment / round-to-cent behaviour — omitted until precision is documented
- Invalid beneficiary format / checksum — omitted until format is documented

### UI

UI coverage is thin. Detailed validation stays at API level. Do not duplicate TA-V01, TA-V02, TA-V03, TA-B01 or TA-B02 in the UI.

#### TA-U01 — Confirmation shown after valid submit

- **Source:** Story 118 MQ1; Feature 116 confirmation; Story 117 AC5
- **Test intent:** UI shows confirmation after successful submit
- **Expected result:** Confirmation is shown. Exact confirmation copy is not asserted unless it becomes a contractual requirement
- **Automation level:** UI
- **Scenario design:** READY (MQ1)
- **Automation implementation:** `BLOCKED` — depends on TA-P01 controlled test data and an agreed way to observe that confirmation is shown

## Future Coverage / Requirement Clarification

These items remain documented risks or requirement gaps. They are **not** current implementation scenarios because expected behaviour is insufficiently specified.

#### TA-P02 — Optional payment message may be provided

- **Source:** Feature 116 (optional payment message)
- **Why deferred:** No AC in Story 117; not in Story 118; no length or content rules
- **Status:** `NEEDS CLARIFICATION`. Not approved for implementation

#### TA-N07 — Unauthenticated transfer attempt

- **Source:** Feature 116 and AC5 require authentication (access risk)
- **Why deferred:** Reject/redirect/status behaviour is not documented. Do not assume HTTP 401/403
- **Status:** `NEEDS CLARIFICATION`. Not approved for implementation

#### TA-V04 — Amount processed in EUR

- **Source:** Story 117 AC3; Feature 116 amount in EUR
- **Why deferred:** AC3 has no Given/When/Then and no invalid-currency behaviour. Foreign-currency transfers are Feature 116 out of scope. Do not invent a non-EUR payload test or conversion rules
- **Status:** `NEEDS CLARIFICATION`. Not approved for implementation

#### Eligible debit-account selection (no scenario ID)

- **Source:** Feature 116
- **Why deferred:** Not in Story 117 ACs or Story 118. Eligibility rules are not documented
- **Status:** Requirement gap. Do not invent eligibility rules or scenarios

## Test Data Requirements

Do not use real customer data, real credentials, or secrets.

| Need | Why | Documented? |
| --- | --- | --- |
| Authenticated retail test identity | Feature 116, AC5 | No. Identity, auth mechanism, and roles are unspecified |
| Eligible debit account | Feature 116 | No. Eligibility rules unspecified (requirement gap) |
| Beneficiary account number that the business considers valid | MQ1, AC5 | No. Format unspecified. **Do not invent a valid IBAN/BBAN** |
| Positive EUR amount the business considers valid | MQ1, AC5 | No. Only “positive” and “EUR” are documented. Precision unspecified |
| Empty amount / omitted amount field | AC1, MQ2 | Encoding depends on unpublished API payload contract |
| Amount zero as in MQ3 `0.00` | AC2 | Encoding depends on unpublished API payload contract |
| Amount below zero as in MQ4 `-10.00` | AC2 | Encoding depends on unpublished API payload contract |
| Empty beneficiary | AC4, MQ5 | Encoding depends on unpublished API payload contract |
| Confirmation oracle (API field and/or UI observation) | AC5, MQ1 | Unspecified |

Until controlled success-path data is supplied, **TA-P01 and TA-U01 automation implementation remains `BLOCKED`**. TA-V01, TA-V02, TA-V03, TA-B01 and TA-B02 also cannot be implemented until the API endpoint, payload and observable response contract are known.

## Preconditions

- Test environment exposing the application transfer API described in Story 117 technical notes
- For TA-U01 only: digital banking transfer form that uses the same backend validation
- Ability to establish an authenticated retail session (mechanism unspecified)
- Ability to observe documented rejection (transfer cannot be submitted / is not completed) from the API response contract — not from invented message text
- No dependence on production accounts or live customer payments

## Recommended Automation Approach

Story 117 states that validation is exposed through the application transfer API and that the UI uses the same backend validation. The API is therefore the **primary automation layer** for detailed validation. Prefer that layer when it proves the same behaviour.

| Scenario | Layer | Rationale |
| --- | --- | --- |
| TA-V01, TA-V02, TA-V03, TA-B01, TA-B02 | **API** (primary) | Backend validation is exposed on the transfer API; UI uses the same backend. Repeatable without duplicating cases in the UI |
| TA-P01 | **API** (primary) | Acceptance and confirmation return can be proven at API |
| TA-U01 | **UI** (thin) | MQ1 requires confirmation **shown**; not fully proven by API alone |
| TA-P02, TA-N07, TA-V04 | Not in current scope | Future Coverage / insufficient expected behaviour |
| Integration / database / full E2E beyond API + thin UI | Not recommended | No integration contract or data model documented |

Do not duplicate every API validation case in the UI. Do not implement any API scenario before endpoint, payload and observable response behaviour are known.

## Framework / Components

No automation framework, API client, or page objects are established in this repository yet.

Intended stack (repository README): Robot Framework, Playwright, Python, API automation, later CI as a quality gate.

Do not introduce a parallel stack without a justified review decision. Implementation is out of scope for this draft.

## Acceptance Criteria for the TA Story

This draft is complete for review when:

- Proposed scenarios are traced to Feature 116, Story 117, and/or Story 118
- Undocumented rules are listed rather than invented
- Scenario design vs automation-implementation status is explicit

After approval and after implementation blockers are lifted, implementation would be complete when:

- Agreed unblocked scenarios are implemented
- Assertions verify documented rejection or acceptance behaviour, not invented message text or limits
- Tests are repeatable and independent
- Test data is controlled and non-sensitive
- Failures identify scenario and observation (request/response or UI confirmation state)
- Agreed tests can run in CI
- Traceability documentation is updated

## Risks / Dependencies

- Missing API contract (endpoint, payload, observable response) blocks **all** current API implementation, including scenarios whose design is READY
- Success path depends on test-account provisioning that Story 118 already flagged
- Exact error-message wording is **non-blocking** unless it becomes a contractual assertion
- Dual API+UI coverage of the same validation increases cost; UI stays limited to success confirmation
- Eligible debit-account behaviour is a Feature 116 requirement gap; automating only Story 117/118 will not prove eligibility
- Optional payment message and unauthenticated access remain unspecified risks (Future Coverage)
- Environment and CI quality gate are not yet defined in the repository

## Missing / Ambiguous Information

| Item | Why it matters | Affected scenarios | Marker |
| --- | --- | --- | --- |
| Transfer API contract (endpoint, method, payload, observable response) | Automation cannot be implemented without knowing how to call the API and how rejection/acceptance is observed | TA-V01, TA-V02, TA-V03, TA-B01, TA-B02, TA-P01 | Automation implementation `BLOCKED` |
| Successful-transfer test accounts (auth, debit account, valid beneficiary, valid positive EUR amount) | AC5/MQ1 cannot run; do not invent business-valid data | TA-P01, TA-U01 | Automation implementation `BLOCKED` |
| Amount precision / decimal places | Cannot know valid positive samples; MQ `0.00` / `-10.00` are examples only | TA-P01; encoding of TA-B01, TA-B02 | `NEEDS CLARIFICATION` (success path). Does not prevent scenario design of zero/negative rejection |
| Beneficiary-account format | Cannot invent a valid number; empty vs malformed is unspecified | TA-P01 | Success path `BLOCKED`. Empty-beneficiary design (TA-V02) remains READY |
| How confirmation is returned (API) and shown (UI) | Needed to assert AC5 / MQ1 without inventing copy | TA-P01, TA-U01 | Automation implementation `BLOCKED` |
| Eligible debit-account rules | Feature 116 gap; no AC/MQ | No current scenario | Requirement gap. Do not invent rules |
| Optional payment message rules | Feature 116 vs absent AC | TA-P02 | Future Coverage |
| Unauthenticated behaviour | Auth is required but reject behaviour is not | TA-N07 | Future Coverage |
| Currency representation (implicit EUR vs field) | AC3 is not executable as written | TA-V04 | Future Coverage |
| Maximum transfer amount | Cannot design max/boundary tests; must not invent a limit | None in current scope | Omitted until documented |
| Exact validation/error-message wording | Must not invent UI/API message text | TA-V01–TA-V03, TA-B01, TA-B02 | **Non-blocking.** Assert documented rejection behaviour. Contractual exact text would be a later dependency |

## Recommendation

**PARTIALLY BLOCKED**

Scenario design for Story 117 validation ACs (required amount, zero/negative amount, required beneficiary, combined empty fields) is READY. Those tests must verify documented rejection behaviour at the API (primary automation layer). Automation implementation of those scenarios is `BLOCKED` until the API endpoint, payload and observable response contract are known. Exact error-message wording does not block that design.

The success path (TA-P01, TA-U01) is `BLOCKED` without controlled test data. Optional message, unauthenticated access, AC3 currency, and debit-account eligibility are out of current implementation scope (Future Coverage / requirement gaps).

Do not implement automation from this draft until the TA Story is reviewed and implementation blockers are resolved or explicitly deferred.

## Implementation gate

No automation code is included. Implementation requires explicit approval of this TA Story after review.

## Azure DevOps TA Story Summary

**Title:** QA-Automation: Automate Domestic Transfer Validation

**Objective:** Automate domestic transfer validation traced from Feature 116 and Story 117 through Manual QA Story 118. Prove invalid required details cannot complete a transfer, and that a valid transfer is accepted and confirmed, without inventing undocumented rules, limits, formats, messages, or test data.

**Parent Feature:** Feature 116 — Domestic Money Transfer

**Related Development Story:** User Story 117 — DEV: Validate Domestic Transfer Details

**Related Manual QA Story:** User Story 118 — MANUAL QA: Validate Domestic Transfer Validation

**Automation Scope:**

- Primary layer: API (validation exposed through the transfer API; UI uses the same backend)
- Thin UI: confirmation shown on successful submit only, when test data exists
- Do not duplicate API validation cases in the UI

**Scenarios:**

| ID | Scenario | Layer | Scenario design | Automation implementation |
| --- | --- | --- | --- | --- |
| TA-V01 | Missing amount — not submitted | API | READY | BLOCKED — API contract |
| TA-V02 | Missing beneficiary — not submitted | API | READY | BLOCKED — API contract |
| TA-V03 | Missing amount and beneficiary — not submitted | API | READY | BLOCKED — API contract |
| TA-B01 | Zero amount — not submitted | API | READY | BLOCKED — API contract |
| TA-B02 | Negative amount — not submitted | API | READY | BLOCKED — API contract |
| TA-P01 | Valid transfer accepted; confirmation returned | API | READY | BLOCKED — test data + API contract |
| TA-U01 | Confirmation shown | UI | READY | BLOCKED — test data + UI observation |
| TA-P02 | Optional payment message | — | Future Coverage | Not in current scope |
| TA-N07 | Unauthenticated transfer | — | Future Coverage | Not in current scope |
| TA-V04 | Amount processed in EUR | — | Future Coverage | Not in current scope |

**Dependencies / Blockers:**

- Transfer API endpoint, payload and observable response contract (blocks all current API implementation)
- Controlled success-path test data (auth, accounts, valid beneficiary, valid positive EUR amount)
- Eligible debit-account rules remain an undocumented Feature 116 gap (no invented rules)
- Exact error-message text is non-blocking unless it becomes contractual

**Acceptance Criteria:**

- Agreed API validation scenarios assert documented rejection (not invented message text)
- Success API and thin UI confirmation run only with controlled test data
- Tests are repeatable, independent, diagnosable, and CI-capable once unblocked
- Traceability to Feature 116, Story 117 and Story 118 is maintained

**Recommendation:** PARTIALLY BLOCKED
