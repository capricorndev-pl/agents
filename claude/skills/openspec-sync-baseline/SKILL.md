---
name: openspec-sync-baseline
description: Sync the current implemented project state back into baseline OpenSpec specs. Use when a repository already has code, docs, APIs, or runtime behavior that may have drifted from `openspec/specs`, and the agent needs to compare reality to the baseline, identify stale or missing requirements, and update baseline specs to reflect current behavior without introducing future intent.
license: MIT
compatibility: Requires openspec CLI.
metadata:
  author: openspec
  version: "1.0"
  generatedBy: "1.3.0"
---

# OpenSpec Sync Baseline

Update baseline OpenSpec specs so they describe the current system as it exists today.

## Purpose

Use this skill to reconcile `openspec/specs` with current implemented reality.

This skill is for baseline maintenance, not future planning:
- capture what already exists
- remove or revise stale baseline statements
- add missing baseline requirements for already-implemented behavior
- flag conflicts that need user judgment

Do not use this skill to propose new features. Use an OpenSpec change for that.

## Workflow

1. Discover the current baseline.

   Read:
   - `openspec/specs/**/spec.md`
   - relevant project docs such as architecture, stack, ADRs, API docs, or README files when they materially describe current behavior

2. Discover current reality.

   Inspect the sources of truth that reflect implemented behavior:
   - code
   - tests
   - migrations or schemas
   - API contracts
   - operational docs if they describe what already exists

3. Compare baseline against reality.

   Classify findings into:
   - `missing in baseline`: implemented behavior not represented in specs
   - `stale in baseline`: spec says something no longer true
   - `ambiguous`: baseline and implementation differ, but intent is unclear
   - `aligned`: baseline already matches reality

4. Propose baseline updates.

   For each missing or stale area:
   - identify the affected capability
   - decide whether to update an existing spec or add a new baseline capability spec
   - draft requirement/scenario updates that describe current behavior only

5. Ask the user only where implementation truth is unclear.

   Examples:
   - partial implementation with unclear intended baseline
   - contradictory docs and code
   - feature flags or unfinished work where "current behavior" is not obvious

6. Update baseline specs.

   Expected edits:
   - revise `openspec/specs/<capability>/spec.md`
   - add new capability specs only when current implemented behavior clearly warrants them
   - keep requirement names stable when possible to minimize churn

7. Summarize the sync result.

   Report:
   - capabilities updated
   - major mismatches found
   - unresolved conflicts
   - whether follow-up changes or proposals are still needed

## Decision Rules

- Prefer implemented behavior over outdated prose when the implementation is clearly established.
- Prefer tests and runtime contracts over comments.
- Prefer baseline project docs over scattered notes when both match the implementation.
- Do not encode aspirational or planned behavior into baseline specs.
- Do not use a baseline sync to sneak in a product change; if the desired future behavior differs from current reality, create an OpenSpec change instead.
- If a current implementation is obviously incomplete or accidental, stop and ask before turning it into baseline truth.

## Spec Editing Rules

- Keep to OpenSpec baseline spec format:
  - capability folder under `openspec/specs/<capability>/`
  - `spec.md`
  - `### Requirement: ...`
  - at least one `#### Scenario: ...` per requirement
- Write requirements as observable system behavior.
- Keep capability boundaries narrow.
- Update existing capabilities before creating new ones when the behavior clearly belongs there.
- Avoid restating architecture implementation details unless they are externally meaningful behavior.

## Output Pattern

Use this structure while working:

```text
## Syncing Baseline

Capabilities reviewed
- <capability>

Findings
- Missing in baseline: <item>
- Stale in baseline: <item>
- Ambiguous: <item>

Planned updates
- Update <capability>
- Add <capability>
```

After updates, summarize:

```text
Updated capabilities: <list>
Unresolved conflicts: <list or none>
Needs follow-up proposal: yes|no
```

## Guardrails

- Do not create or modify `openspec/changes` artifacts as part of this skill unless the user explicitly asks.
- Do not treat TODOs, placeholders, or dead code as baseline behavior.
- Do not assume docs are current if code and tests disagree.
- Keep the skill general: adapt the comparison sources to the repository you are in.
