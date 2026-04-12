---
name: feature-verification
description: Verify a completed feature implementation against its spec and PRD. Use to check for gaps and unmet requirements.
---

# Feature Verification

## Overview

Verify that an implemented feature matches its feature spec and the relevant PRD requirements. Focus on evidence in the codebase and tests, then report what is implemented, missing, partial, or divergent.

## Workflow

1. Locate the source documents.
2. Identify the feature and its spec.
3. Extract a verification checklist from the spec.
4. Explore the codebase for the implementation.
5. Compare implementation against the checklist.
6. Verify FR coverage when applicable.
7. Check whether tests exist for the expected areas.
8. Report findings without prescribing fixes.

## Locate The Source Documents

Look for:

- PRD: `docs/prd.md`
- Feature map: `docs/feature-map.md`
- Feature specs directory: `docs/features/`

If any of these are missing, stop and tell the user exactly which document or directory is unavailable.

## Confirm The Feature

Ask which feature should be verified unless the user has already named it.

Locate the corresponding spec in `docs/features/`.

If the feature spec references FR identifiers from the PRD, note them for later verification.

## Build The Verification Checklist

Read the feature spec thoroughly and extract a concrete checklist of verifiable requirements from:

- User Stories
- Detailed Behaviour
- Data Model
- Access Control
- Integration Points
- Constraints & Edge Cases
- Testing Strategy

Translate the spec into implementation-facing checks. Each checklist item should be something you can verify in the codebase or test suite.

Present this checklist to the user before moving into codebase review.

## Explore The Codebase

Use the spec to guide the search:

- search for entity names, route paths, API identifiers, events, and key terms from the spec
- read the relevant implementation files
- inspect related tests

Be thorough but stay scoped to the feature. Follow the spec structure instead of scanning the entire repository without direction.

## Compare Implementation Against The Spec

For each checklist item, classify the result as one of:

- `Implemented`
- `Partially implemented`
- `Missing`
- `Deviates`

Use evidence from the codebase when assigning the status.

If something deviates from the spec, describe the difference factually without arguing whether it is better or worse.

## Verify FR Coverage

If the feature spec references FR identifiers from the PRD, check whether each referenced FR appears to be addressed by the implementation.

List any FRs that are not covered.

## Check Test Presence

Verify whether tests exist for the feature and whether they cover the expected areas:

- key behaviors from the spec
- edge cases from Constraints & Edge Cases
- any specific concerns called out in Testing Strategy

Do not judge test quality in depth. The goal here is presence and rough alignment, not a full test audit.

## Reporting Format

Present findings using this structure:

### Verification Summary

**Feature:** [name]
**Spec:** [path]
**Status:** [All requirements met / Gaps found]

### Checklist

For each requirement:

- [Implemented / Partially implemented / Missing / Deviates] — [requirement description]
  - [brief note on what was found, or what is missing/different]

### FR Coverage

- FR-XYZ-1: [Covered / Not covered]
- ...

### Test Coverage

- [summary of what is tested and what is not]

### Gaps

[consolidated list of missing or incomplete items, or `No gaps found.`]

## Rules

- do not suggest fixes or implementation approaches
- do not rewrite the feature spec
- report only what matches, what is missing, and what deviates
- keep findings tied to evidence from the codebase and tests
