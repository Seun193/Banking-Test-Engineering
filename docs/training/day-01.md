# Day 1 — Requirement to TA Story

## Objective

Practise using governed AI assistance to convert source requirements into a reviewable Test Automation Story.

## Inputs

- Feature 1001
- Development Story 1011
- Manual QA Story 1021
- Cursor rules in `.cursor/rules/`

## Cursor exercise

Ask Cursor:

> Read Feature 1001, Development Story 1011 and Manual QA Story 1021 in this repository. Apply the Test Automation Story Generator rule. Create the TA Story only. Do not implement automation code. Identify requirement gaps and preserve traceability.

## What to review manually

- Did Cursor read all three source work items?
- Did it map scenarios to acceptance criteria?
- Did it notice the unresolved maximum amount?
- Did it notice the unresolved amount precision?
- Did it notice beneficiary-format ambiguity?
- Did it avoid inventing exact error messages?
- Did it distinguish API vs UI automation?
- Did it mark blocked scenarios appropriately?
- Did it avoid writing code before review?

## Interview-ready takeaway

“I use AI as a governed engineering assistant. Before generating automation, I require it to trace tests to requirements, compare development and manual-QA coverage, identify missing information, and produce a reviewable TA story. I do not allow the model to invent business rules.”
