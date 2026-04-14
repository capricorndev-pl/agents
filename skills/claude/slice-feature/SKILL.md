---
name: slice-feature
description: Break a feature spec into a small set of vertical implementation slices and recommend the best next slice to build. Use to turn a feature spec into 3-7 end-to-end slices with done criteria and a handoff block.
allowed-tools: Read, Glob
---

# Slice Feature

You are deriving implementation slices from a feature spec. Always adhere to any rules or requirements set out in CLAUDE.md when responding.

## Step 1. Read only the needed spec context

Start with the target feature spec (look in `docs/features/`).

Read adjacent documents only when they materially affect slicing:

- the matching feature entry in `docs/feature-map.md` for dependencies or boundaries
- the PRD (`docs/prd.md`) only when the feature spec is too vague to infer a sensible implementation order

Do not load unrelated specs by default. If expected documents are missing, inform the user and ask how to proceed.

## Step 2. Extract candidate user outcomes

Convert the feature spec into a short set of user-visible or externally verifiable outcomes.

Prefer outcomes such as:

- a role can perform an action
- a run moves through one lifecycle transition
- data is accepted, transformed, or exported in one concrete way
- a validation rule is enforced with an observable result

Do not slice by technical layer alone such as "backend", "frontend", or "database".

## Step 3. Form vertical slices

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

### Good slice characteristics

Prefer slices that can be demonstrated quickly and tested with a few focused cases. A good slice usually has:

- one clear outcome
- one main rule set
- one primary dependency path
- a contained failure surface

### Bad slice patterns

Avoid slices that are:

- purely horizontal, such as "implement backend"
- too broad, such as "implement manual correction"
- mostly infrastructure with no visible outcome
- blocked by too many unresolved dependencies at once

### Prerequisites and blockers

Call out prerequisites explicitly. If a slice is blocked by another feature or missing system capability, say so instead of forcing an artificial order. When possible, separate the blocked slice from an earlier unblocked slice that still provides value.

## Step 4. Choose the best next slice

Recommend one next slice to build first.

Prefer the slice that:

- has the smallest blast radius
- exercises the core path of the feature
- has the fewest unresolved policy decisions
- creates useful leverage for later slices

## Step 5. Produce a handoff block

End with a compact block that can be given directly to the coding agent.

Use this output structure:

```
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

## Boundaries

- Default to console output only. Do not create a file unless the user explicitly asks for one.
- If the user explicitly asks for a file, write a compact markdown note rather than a large planning document.
