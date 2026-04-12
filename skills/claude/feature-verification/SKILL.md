---
name: feature-verification
description: Verify a completed feature implementation against its spec and PRD. Use to check for gaps and unmet requirements.
allowed-tools: Read, Glob, Bash(git log:*), Bash(git diff:*), Bash(grep:*), Bash(find:*)
---

# Feature Verification

You are verifying that a feature implementation matches its specification. Always adhere to any rules or requirements set out in CLAUDE.md when responding.

## Step 1. Locate project documents

Look for:
- PRD: `docs/prd.md`
- Feature map: `docs/feature-map.md`
- Feature specs: `docs/features/`

If any of these are missing, inform the user and ask how to proceed.

## Step 2. Identify the feature

Ask the user which feature to verify. Locate its spec in `docs/features/`. If the feature spec references FR identifiers from the PRD, note them for later verification.

## Step 3. Understand what was specified

Read the feature spec thoroughly. Extract a checklist of verifiable requirements from:
- User Stories — each story implies functionality that should exist
- Detailed Behaviour — each described behaviour should be implemented
- Data Model — entities, fields, relationships should exist
- Access Control — permission rules should be enforced
- Integration Points — routes, APIs, events should be wired up
- Constraints & Edge Cases — edge cases should be handled
- Testing Strategy — if specific testing concerns were noted, tests should exist

Present this checklist to the user before proceeding to codebase review.

## Step 4. Explore the codebase

Search the codebase for the implementation. Use the spec as a guide for where to look:
- Glob and grep for entity names, route paths, API identifiers, and key terms from the spec
- Read relevant files to understand what was actually implemented
- Look for test files related to the feature

Be thorough but focused — follow the spec's structure to guide your exploration rather than scanning the entire codebase.

## Step 5. Diff against spec

For each item in the checklist from Step 3, assess:
- **Implemented** — the requirement is met
- **Partially implemented** — some aspects are present but incomplete
- **Missing** — no evidence of implementation
- **Deviates** — implemented but differently than specified

If deviations are found, note what differs without judging whether the deviation is an improvement or a problem — that's for the user to decide.

## Step 6. Verify FR coverage

If the feature spec references FR identifiers from the PRD, check whether each referenced FR is addressed by the implementation. List any unaddressed FRs.

## Step 7. Check for tests

Verify that tests exist for the feature. Assess:
- Do tests cover the key behaviours described in the spec?
- Are edge cases from the Constraints & Edge Cases section tested?
- If the Testing Strategy section noted specific concerns, are they addressed?

Do not assess test quality or coverage depth — just whether tests exist for the expected areas.

## Step 8. Report

Present findings in the following format:

### Verification Summary

**Feature:** [name]
**Spec:** [path]
**Status:** [All requirements met / Gaps found]

### Checklist

For each requirement:
- [Implemented / Partially implemented / Missing / Deviates] — [requirement description]
  - [Brief note on what was found, or what's missing/different]

### FR Coverage

- FR-XYZ-1: [Covered / Not covered]
- ...

### Test Coverage

- [Summary of what's tested and what's not]

### Gaps

[Consolidated list of missing or incomplete items, if any. If no gaps — "No gaps found."]

---

Do not suggest fixes or implementation approaches. Only report what matches, what's missing, and what deviates.