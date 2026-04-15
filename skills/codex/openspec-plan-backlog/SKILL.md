---
name: openspec-plan-backlog
description: Shape a prioritized backlog of OpenSpec changes from current specs, docs, code, and active work. Use when Codex needs to suggest what proposals should exist next, how to split work into coherent changes, and in what order those changes should be tackled based on dependencies, risk, and delivery value.
---

# OpenSpec Plan Backlog

Create a practical backlog of OpenSpec changes from the current project state.

## Purpose

Use this skill to turn broad product scope or a large body of current work into a proposal-sized backlog.

This skill is advisory:
- identify candidate changes
- group work into coherent proposal units
- order those units by dependency and delivery value
- highlight missing foundations, sequencing risks, and overly large proposals

Do not use this skill to implement changes. Do not generate code.

## Workflow

1. Read planning inputs.

   Use the sources that exist in the repository, such as:
   - PRD or roadmap docs
   - `openspec/specs/**/spec.md`
   - active changes under `openspec/changes/`
   - feature maps, architecture docs, stack docs
   - current codebase state when relevant

2. Determine the planning scope.

   Identify whether the user wants:
   - a full backlog
   - a next-wave backlog
   - backlog refinement for one area
   - a dependency-ordered implementation sequence

3. Extract candidate work items.

   Look for:
   - missing capabilities implied by product docs
   - baseline specs with no corresponding implementation plan
   - oversized active changes that should be split
   - duplicate or overlapping change ideas
   - enabling work needed before feature work can proceed

4. Shape proposal-sized changes.

   Each proposed change should be:
   - coherent
   - reviewable
   - implementable in a bounded time window
   - small enough that tasks can map to execution work cleanly

5. Order the backlog.

   Use these priorities:
   - dependencies first
   - risk reduction early
   - thin end-to-end slices before broad expansion
   - polish and optimization after functional capability

6. Mark each proposal candidate.

   For each item provide:
   - change name
   - one-line purpose
   - why now
   - dependencies
   - likely affected capabilities
   - notes on splitting or combining if relevant

7. Call out uncertainty.

   Ask the user only when backlog shape depends on product strategy that cannot be inferred safely, such as:
   - competing priorities
   - staffing constraints
   - demo deadlines
   - whether to optimize for foundations or feature visibility

## Decision Rules

- Prefer one meaningful outcome per change over catch-all proposals.
- Prefer enabling foundations only when they unlock immediate downstream work.
- Avoid backlog items that are just technical layers with no delivery reason unless they are true blockers.
- Split a proposal when it spans unrelated capabilities or cannot be reviewed clearly.
- Merge proposals when separation would create artificial overhead without reducing risk.
- Do not create backlog items for already completed work unless follow-up change management is required.

## Output Pattern

Use this structure:

```text
## Backlog Proposal

Recommended order
1. <change-name> — <purpose>
2. <change-name> — <purpose>

Details
- <change-name>
  Why now: <reason>
  Depends on: <items or none>
  Affects: <capabilities/docs>
```

If helpful, also include:

```text
Not recommended yet
- <change-name> — <why it should wait>

Needs clarification
- <topic>
```

## Guardrails

- Keep the skill general and repository-agnostic.
- Do not create `openspec/changes/...` automatically unless the user explicitly asks.
- Do not confuse baseline sync work with future proposals.
- Keep backlog planning separate from implementation tasks.
- Favor actionable backlog slices over exhaustive theoretical decomposition.