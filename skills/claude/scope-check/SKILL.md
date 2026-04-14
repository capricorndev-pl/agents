---
name: scope-check
description: Check whether a requested code or product change aligns with the repository's PRD, feature map, and feature specs before implementation. Use to classify a request as aligned, spec drift, scope drift, or unclear.
allowed-tools: Read, Glob, Write
---

# Scope Check

You are performing a lightweight pre-implementation scope gate. Always adhere to any rules or requirements set out in CLAUDE.md when responding.

## Step 1. Identify the target scope

Determine the most likely target feature from the user request.

If the target feature is explicit, use it. If it is implicit, infer the most likely feature from the repository's feature map and nearby specs. If multiple features are plausible and the ambiguity affects implementation, classify as `unclear`.

## Step 2. Read in the cheapest useful order

Protect the context window. Do not load all product documents by default.

Use this retrieval order:

1. Read the relevant feature spec if one already exists (look in `docs/features/`).
2. Read only the matching feature entry from `docs/feature-map.md`.
3. Read only the relevant PRD section (from `docs/prd.md`) if scope is uncertain, disputed, or possibly broader than the feature spec.
4. Read adjacent specs only when the request crosses ownership boundaries.

If any expected documents are missing, inform the user and ask how to proceed.

## Step 3. Classify the request

Return exactly one classification:

- `aligned` — the requested change matches both product scope and the current feature spec
- `spec_drift` — the change appears consistent with product scope but not with the current feature spec
- `scope_drift` — the change appears to expand or conflict with the PRD or feature map scope
- `unclear` — the target feature, ownership boundary, or source documents are too ambiguous to classify confidently

## Step 4. Explain the result

Keep the output compact and decision-oriented. State:

- target feature
- requested change in one sentence
- classification
- why it was classified that way
- what the user can do next

Use this format:

```
Feature: <feature id or name>
Change: <one-sentence description>
Classification: <aligned | spec_drift | scope_drift | unclear>
Why: <brief explanation>
Next step: <recommended action>
```

### Classification-specific guidance

**aligned** — Proceed directly to implementation unless the user asked only for analysis.

**spec_drift** — Offer the user these options:
- update the existing feature spec
- create a new feature spec if the work introduces a distinct scope boundary
- proceed intentionally without updating docs, but label it as drift

**scope_drift** — Do not silently implement against scope drift. Surface the mismatch first so the user can make an informed product decision.

**unclear** — Ask only the minimum clarifying question needed, or propose the most likely options with the tradeoff between them.

## Step 5. Create a change-intent note only when it helps

Do not create a file by default.

Create a change-intent note only when at least one of these is true:

- the request is `spec_drift`
- the request is `scope_drift`
- the request is `unclear` and the user wants the decision captured
- the change will likely be implemented later, after a product or spec decision
- the change spans multiple features and a durable ownership note will avoid repeated re-analysis

If a durable note is useful, use the template from `assets/change-intent-template.md` and save it to a repo path chosen by the user or established repo workflow.

## Boundaries

- Do not rewrite the PRD or specs automatically as part of the scope check.
- Do not create a change-intent note just to mirror a conversation that is already resolved and low-risk.
- Do not load the full PRD or every feature spec unless the request truly spans a broad product area.
