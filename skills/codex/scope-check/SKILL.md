---
name: scope-check
description: Check whether a requested code or product change aligns with the repository's PRD, feature map, and feature specs before implementation. Use when Codex needs to classify a request as aligned, spec drift, scope drift, or unclear; when deciding whether a spec update or a new spec is needed; or when optionally creating a lightweight change-intent note from a bundled template.
---

# Scope Check

## Overview

Use this skill to perform a lightweight pre-implementation scope gate. Keep the read set small, classify the request, and only create a durable change-intent file when the mismatch or decision is worth preserving.

This skill follows the Agent Skills Open Standard layout: operational instructions live in `SKILL.md`, UI metadata lives in `agents/openai.yaml`, and reusable output templates live in `assets/`.

## Workflow

### 1. Identify the Target Scope

Determine the most likely target feature from the user request.

If the target feature is explicit, use it. If it is implicit, infer the most likely feature from the repository's feature map and nearby specs. If multiple features are plausible and the ambiguity affects implementation, classify as `unclear`.

### 2. Read in the Cheapest Useful Order

Protect the context window. Do not load all product documents by default.

Use this retrieval order:

1. Read the relevant feature spec if one already exists.
2. Read only the matching feature entry from `docs/feature-map.md`.
3. Read only the relevant PRD section if scope is uncertain, disputed, or possibly broader than the feature spec.
4. Read adjacent specs only when the request crosses ownership boundaries.

### 3. Classify the Request

Return exactly one classification:

- `aligned`: the requested change matches both product scope and the current feature spec
- `spec_drift`: the change appears consistent with product scope but not with the current feature spec
- `scope_drift`: the change appears to expand or conflict with the PRD or feature map scope
- `unclear`: the target feature, ownership boundary, or source documents are too ambiguous to classify confidently

### 4. Explain the Result Briefly

Keep the output compact and decision-oriented. State:

- target feature
- requested change in one sentence
- classification
- why it was classified that way
- what the user can do next

Prefer a short block such as:

```md
Feature: CF-1
Change: Allow operational users to export intermediate checkpoint data
Classification: scope_drift
Why: The PRD and feature map reserve intermediate export for administrators
Next step: Reject, or update the PRD/feature map/spec before implementation
```

### 5. Create a Change-Intent Note Only When It Helps

Do not create a file by default.

Create a change-intent note only when at least one of these is true:

- the request is `spec_drift`
- the request is `scope_drift`
- the request is `unclear` and the user wants the decision captured
- the change will likely be implemented later, after a product or spec decision
- the change spans multiple features and a durable ownership note will avoid repeated re-analysis

If a durable note is useful, copy the template from `assets/change-intent-template.md` into a repo path chosen by the user or established repo workflow.

## Decision Rules

### Treat as `aligned`

Use `aligned` when the request clearly fits the existing feature contract and does not expand product scope.

Proceed directly to implementation unless the user asked only for analysis.

### Treat as `spec_drift`

Use `spec_drift` when the PRD still supports the request, but the current feature spec does not describe it or explicitly excludes it.

Offer the user these options:

- update the existing feature spec
- create a new feature spec if the work introduces a distinct scope boundary
- proceed intentionally without updating docs, but label it as drift

### Treat as `scope_drift`

Use `scope_drift` when the request would broaden, contradict, or bypass the PRD or feature map.

Do not silently implement against scope drift. Surface the mismatch first so the user can make an informed product decision.

### Treat as `unclear`

Use `unclear` when the source documents do not provide enough signal to assign clean ownership or scope.

Ask only the minimum clarifying question needed, or propose the most likely options with the tradeoff between them.

## Boundaries

Do not rewrite the PRD or specs automatically as part of the scope check.

Do not create a change-intent note just to mirror a conversation that is already resolved and low-risk.

Do not load the full PRD or every feature spec unless the request truly spans a broad product area.

## Asset

Use `assets/change-intent-template.md` when a durable note is needed. Keep the filled note short. It is a decision record, not a design document.