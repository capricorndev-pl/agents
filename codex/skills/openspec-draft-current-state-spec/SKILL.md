---
name: openspec-draft-current-state-spec
description: Draft or update an OpenSpec `spec.md` to reflect the current implemented state of this app by reconciling docs with the codebase. Use when the user asks to "spec the current state", "map a docs section into OpenSpec", "reconcile docs with code", or otherwise wants a current-state capability spec under `openspec/specs/`.
---

# OpenSpec Draft Current State Spec

Use this skill to author or revise `openspec/specs/<capability>/spec.md` files that describe what the app currently does.

## Scope

This skill is for current-state capability specs only.

Use it for:

- existing implemented features
- reconciling a doc section with code
- documenting actual route, API, model, auth, or UI behavior

Do not use it for:

- proposals
- designs
- tasks
- desired future behavior
- implementation work

## Workflow

1. Identify the target capability name and spec path.
2. Read the source docs the user referenced.
3. Inspect the actual code paths, config, collections, routes, and components that implement the behavior.
4. Read nearby OpenSpec specs to avoid overlap and keep terminology consistent.
5. If the topic touches durable architecture concerns called out in `AGENTS.md`, read the relevant ADRs in `docs/adrs/` first.
6. Draft or update `openspec/specs/<capability>/spec.md` in the existing OpenSpec style.
7. Validate with `openspec validate --specs <capability>`.
8. Report the important drift between docs and implementation.

## Ground Rules

- Trust code over docs when they diverge.
- Treat docs as intent or hints, not proof.
- Do not spec aspirational, planned, or placeholder behavior as if it exists.
- If the implementation is partial, say so plainly.
- If behavior is unclear from both code and docs, say that instead of inventing certainty.

## Writing Rules

- Follow the established format:
  - `## Purpose`
  - `## Requirements`
  - `### Requirement: ...`
  - `#### Scenario: ...`
  - `- **WHEN** ...`
  - `- **THEN** ...`
- Write in current-state language such as:
  - `The application SHALL currently...`
  - `The current implementation SHALL...`
  - `The page SHALL expose...`
- Capture only implemented behavior that you can support from code.
- Keep requirements grouped by real capability areas such as routes, data flow, access model, query behavior, UI behavior, and edge cases.

## Drift Handling

When docs and code differ:

- put the code-backed behavior in the spec
- mention the drift in the user-facing summary
- only include the docs-backed behavior if the code also supports it

Examples of drift worth calling out:

- visible UI that is still a placeholder
- filtering logic that behaves differently from the feature doc
- page size, sorting, or pagination differences
- auth or access nuances that the design doc oversimplifies
- independent config surfaces added after the original doc was written

## Finish Line

Before finishing:

- ensure the spec file exists at the intended path
- run `openspec validate --specs <capability>`
- summarize the key reconciled drift points in plain language
