---
name: openspec-resolve-questions
description: Resolve unanswered design, scope, naming, and implementation-readiness questions inside an OpenSpec change before `/opsx:apply`. Use when a change contains an `Open Questions` section, ambiguous assumptions, unresolved tradeoffs, or tasks that cannot be executed cleanly until decisions are made.
license: MIT
compatibility: Requires openspec CLI.
metadata:
  author: openspec
  version: "1.0"
  generatedBy: "1.3.0"
---

# OpenSpec Resolve Questions

Resolve unresolved questions inside one OpenSpec change and make the change implementation-ready.

## Workflow

1. Select the change.

   If the user names a change, use it. Otherwise infer it from context only when it is unambiguous.

2. Read the change artifacts that can contain unresolved questions:
   - `proposal.md`
   - `design.md`
   - `tasks.md`
   - `specs/**/spec.md`

3. Extract unresolved items.

   Look for:
   - `## Open Questions`
   - explicit question bullets
   - TODO markers
   - contradictory statements across artifacts
   - tasks blocked by missing decisions

4. Classify each item:
   - `blocking`: must be resolved before `/opsx:apply`
   - `non_blocking`: can proceed with an explicit assumption
   - `follow_up`: intentionally deferred to a later change

5. Propose concrete resolutions.

   For each item:
   - give 1 recommended answer
   - give brief tradeoff notes only when they matter
   - state which files must change

6. Ask the user only for decisions that cannot be made safely from repo context.

   Keep questions short and direct. Prefer a recommended default.

7. Update artifacts consistently after decisions are made.

   Expected edits:
   - replace open questions in `design.md` with decisions or assumptions
   - adjust `proposal.md` if scope changed
   - adjust `tasks.md` so implementation matches the decision
   - adjust change specs if the decision alters required behavior

8. End with a readiness check.

   Report:
   - resolved items
   - deferred items
   - whether the change is ready for `/opsx:apply`

## Decision Rules

- Prefer explicit decisions over leaving questions open.
- If the answer is already implied by baseline specs, stack docs, architecture docs, or the change proposal, adopt that answer and update the artifacts.
- Use assumptions only when the decision is minor and unlikely to cause rework.
- If a question would materially change scope or behavior, reflect that in specs or proposal text, not only in `design.md`.
- Do not silently invent product behavior that conflicts with baseline OpenSpec specs.

## Output Pattern

Use this structure while working:

```text
## Resolving Questions: <change-name>

Blocking
- <question>
  Recommendation: <decision>
  Files to update: <files>

Non-blocking
- <question>
  Recommendation: <assumption>
  Files to update: <files>

Deferred
- <question>
  Recommendation: leave for later change
```

After updates, summarize:

```text
Resolved: <count>
Deferred: <count>
Ready for /opsx:apply: yes|no
```

## Guardrails

- Keep the skill focused on question resolution, not implementation.
- Do not run `/opsx:apply` as part of this skill.
- Do not leave stale open questions behind after converting them into decisions.
- When one answer affects multiple artifacts, update all of them in the same pass.
