---
name: openspec-apply-selected-tasks
description: Implement only selected tasks from an OpenSpec change. Use when the user wants to work on specific task numbers, sections, ranges, or a small subset like "1.1", "2.3-2.5", or "section 5" instead of applying the whole change.
license: MIT
compatibility: Requires openspec CLI.
metadata:
  author: Holiport
  version: "1.1"
  derivedFrom: "openspec-apply-change"
---

Implement only the tasks the user selected from an OpenSpec change.

Prefer this skill over `openspec-apply-change` (and its `/opsx:apply` slash command) when the request names task numbers, a section, a range, or a narrow subset of work.

**Input**: A change name is optional if it can be inferred. A task selection is required unless it can be inferred from the user's request.

## Accepted Task Selectors

- task labels from `tasks.md`: `1.1`, `2.3`, `7.10`
- CLI task IDs from `openspec instructions apply --json`: `1`, `8`, `31-35`
- section selectors: `section 5`, `5.*`
- mixed lists and ranges: `1.1, 1.2, 2.1-2.3`
- short text matches from task descriptions when numbering is unclear

If the user gives both task labels and a section, prefer the more specific selection.

## Workflow

1. **Resolve the change**

   If the user named a change, use it. Otherwise:
   - infer it from the conversation if there is one obvious candidate
   - auto-select if only one active change exists
   - if still ambiguous, run `openspec list --json` and use the **AskUserQuestion tool** to let the user pick

   Always announce: `Using change: <name>` and how to override (for example, by naming a different change in the next message).

2. **Get the apply data**

   ```bash
   openspec status --change "<name>" --json
   openspec instructions apply --change "<name>" --json
   ```

   Use the JSON from `openspec instructions apply` as the source of truth for:
   - task IDs
   - task descriptions
   - done vs pending state
   - overall progress
   - context file paths

   Handle states:
   - `blocked`: explain what is missing and stop
   - `all_done`: report that everything is already complete and stop
   - otherwise continue

3. **Resolve the selected tasks**

   Convert the user's task selectors into a concrete ordered task list.

   Use this strategy:
   - first, match directly against the CLI task list when the selector is a CLI ID
   - then map human task labels like `1.1` or `7.10` by matching the numeric prefix at the start of each task description
   - for a section selector like `section 5` or `5.*`, select every task whose description starts with `5.`
   - for a range like `2.3-2.5`, expand it to the matching task labels in that section
   - only use text matching as a fallback, and use the **AskUserQuestion tool** to confirm if there is more than one plausible match

   If no task selection can be resolved, use the **AskUserQuestion tool** to clarify and stop until answered.

   Before implementing, announce the normalized selection with both the task label and CLI task ID when helpful.

4. **Read only the focused context**

   Do **not** load the entire `tasks.md` into context by default.

   Read only:
   - the selected task lines and their nearest section heading from `tasks.md`
   - the minimum proposal, design, and spec context needed to implement those tasks
   - the code paths directly touched by the selected tasks

   Preferred approach:
   - start from `openspec instructions apply --json`
   - use targeted Grep / Read in `tasks.md`, `design.md`, and relevant `specs/**/*.md`
   - expand outward only when a selected task depends on more context

   Only read the full `tasks.md` if the selection cannot be resolved any other way.

5. **Check for prerequisite blockers**

   Before editing code, sanity-check whether the selected tasks depend on unfinished earlier tasks outside the selection.

   If a dependency is clearly blocking, pause and explain:
   - which selected task is blocked
   - which missing task appears to be the prerequisite
   - whether the user should include that prerequisite in scope

   Do not silently implement a broad prerequisite chain unless it is small, obvious, and strictly necessary to complete the selected tasks safely.

6. **Implement only the selected tasks**

   Work through the selected tasks in order. Use the **TodoWrite tool** to track the selected subset when it contains 3 or more tasks.

   For each selected task:
   - show which exact task is being worked on
   - make the minimal code changes needed
   - keep edits scoped to the selected task set
   - update only that task's checkbox in `tasks.md` when it is truly complete (`- [ ]` → `- [x]`)

   Do **not** continue to unrelated pending tasks after the selected subset is done.

   Pause if:
   - the task is ambiguous
   - the task uncovers a design problem (suggest updating `proposal.md` / `design.md` / specs instead of patching around it)
   - the selected scope is insufficient to ship safely
   - a required command or validation step fails

7. **Report both subset progress and overall progress**

   On completion or pause, show:
   - the selected tasks completed this session
   - any selected tasks left incomplete
   - the overall OpenSpec progress from the apply JSON
   - the next best follow-up task selectors if more work is needed

## Output Shape

### During Implementation

```text
## Implementing Selected Tasks: <change-name> (schema: <schema-name>)

Selected tasks:
- 1.1 (CLI 1): Add `pageLayout` select field
- 1.2 (CLI 2): Add `visibleModules` group field

Working on 1.1 (CLI 1): Add `pageLayout` select field
[...implementation...]
✓ 1.1 complete

Working on 1.2 (CLI 2): Add `visibleModules` group field
[...implementation...]
✓ 1.2 complete
```

### On Completion

```text
## Selected Tasks Complete

Change: <change-name>
Schema: <schema-name>
Completed this session:
- [x] 1.1 ...
- [x] 1.2 ...

Overall progress: 2/55 tasks complete

Selected scope is done. Ask for another task range to continue.
```

### On Pause

```text
## Selected Task Work Paused

Change: <change-name>
Schema: <schema-name>
Selected tasks:
- 1.5 ...
- 1.6 ...

Issue:
Task 1.5 depends on schema changes from 1.1 and 1.2, which are not complete yet.

Suggested next selectors:
- 1.1-1.2
- 1.1-1.6
```

## Guardrails

- Keep the task selection narrow and explicit.
- Treat `openspec instructions apply --json` as the task inventory source of truth.
- Use `tasks.md` primarily to map labels and update the exact checkboxes you completed.
- Do not read or summarize the entire backlog unless the user asked for a broad pass.
- Do not auto-expand the scope from "a few tasks" to "the whole section" without saying so.
- If a selector is ambiguous, use the **AskUserQuestion tool** before editing.
- If selected tasks are already complete, say so and stop.
- If the user did not select tasks and wants broad implementation, use `openspec-apply-change` (or `/opsx:apply`) instead.

## Fluid Workflow Integration

This skill supports the "actions on a change" model and can be invoked anytime:

- Before all artifacts are done, if tasks exist for the selected subset
- After partial implementation, to pick off a specific slice
- Interleaved with other actions on the same change

If implementation of a selected task reveals a design issue, pause and suggest updating the artifacts (`proposal.md`, `design.md`, specs) rather than working around it in code.
