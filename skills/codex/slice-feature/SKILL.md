---
name: slice-feature
description: Break a feature spec into a small set of sensible vertical implementation slices and recommend the best next slice to build. Use when Codex needs to turn a feature spec into 3-7 end-to-end slices, identify prerequisites and blockers, produce done criteria, and generate a copy-pasteable implementation handoff without creating persistent documents by default.
---

# Slice Feature

## Overview

Use this skill to derive implementation slices from a feature spec without turning the exercise into a large planning artifact. Default to console output only: candidate slices, one recommended next slice, and a compact handoff block for the coding agent.

## Workflow

### 1. Read Only the Needed Spec Context

Start with the target feature spec.

Read adjacent documents only when they materially affect slicing:

- the matching feature entry in `docs/feature-map.md` for dependencies or boundaries
- the PRD only when the feature spec is too vague to infer a sensible implementation order

Do not load unrelated specs by default.

### 2. Extract Candidate User Outcomes

Convert the feature spec into a short set of user-visible or externally verifiable outcomes.

Prefer outcomes such as:

- a role can perform an action
- a run moves through one lifecycle transition
- data is accepted, transformed, or exported in one concrete way
- a validation rule is enforced with an observable result

Do not slice by technical layer alone such as "backend", "frontend", or "database".

### 3. Form Vertical Slices

Propose 3-7 slices. Each slice should be:

- end-to-end enough to verify
- small enough for one focused implementation session
- narrow enough to avoid bundling multiple major decisions
- aligned to one primary outcome

Each slice must include:

- `name`
- `user outcome`
- `why this is a good boundary`
- `prerequisites`
- `done criteria`
- `risks or ambiguities`

### 4. Choose the Best Next Slice

Recommend one next slice to build first.

Prefer the slice that:

- has the smallest blast radius
- exercises the core path of the feature
- has the fewest unresolved policy decisions
- creates useful leverage for later slices

### 5. Produce a Handoff Block

End with a compact block that can be given directly to the coding agent.

Include:

- target feature
- chosen slice
- scope boundary
- done criteria
- instruction to stop on spec drift or missing decisions

## Decision Rules

### Good Slice Characteristics

Prefer slices that can be demonstrated quickly and tested with a few focused cases.

A good slice usually has:

- one clear outcome
- one main rule set
- one primary dependency path
- a contained failure surface

### Bad Slice Patterns

Avoid slices that are:

- purely horizontal, such as "implement backend"
- too broad, such as "implement manual correction"
- mostly infrastructure with no visible outcome
- blocked by too many unresolved dependencies at once

### Prerequisites and Blockers

Call out prerequisites explicitly.

If a slice is blocked by another feature or missing system capability, say so instead of forcing an artificial order. When possible, separate the blocked slice from an earlier unblocked slice that still provides value.

## Output Format

Default to console output only. Do not create a file unless the user explicitly asks for one.

Use this structure:

```md
Feature: <feature id or name>

Slices:
1. <slice name>
   Outcome: <user-visible or externally verifiable result>
   Why: <why this is a good boundary>
   Prerequisites: <none / list>
   Done: <finish condition>
   Risks: <main ambiguity or blocker>

Recommended next slice:
<slice name>
Reason: <why it should be built first>

Implementation handoff:
Implement <slice name> for <feature>.
Stay within the feature spec.
Done criteria:
- <criterion>
- <criterion>
Stop and report any spec drift or missing decisions before going further.
```

If the user explicitly asks for a file, write a compact markdown note rather than a large planning document.