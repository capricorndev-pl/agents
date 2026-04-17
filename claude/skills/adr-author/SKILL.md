---
name: adr-author
description: Produces Architecture Decision Records (ADRs) — single Markdown files capturing one significant technical decision with its context, options, outcome, and consequences. Use this skill whenever the user wants to write, draft, scaffold, or add an ADR, a "decision record", a "design decision doc", a "tech decision doc", or asks to document a choice between technical options (database, framework, library, deployment target, protocol, pattern). Also use when the user mentions MADR, Nygard ADR, Y-statement, or a `docs/adr/` or `docs/decisions/` directory. Prefer this skill over writing freeform prose whenever the underlying task is "capture why we chose X over Y".
license: MIT
metadata:
  version: "0.1.0"
  author: piotr
---

# ADR Author

An Architecture Decision Record (ADR) captures **one** architecturally significant decision: what was decided, why, what was rejected, and what the consequences are. This skill produces ADRs as Markdown files following established community templates.

## When this skill applies

Trigger whenever the user wants to:

- Write, draft, or scaffold an ADR / decision record / design decision.
- Document a choice between technical options (DB, framework, protocol, hosting, auth approach, etc.).
- Add an entry to an existing `docs/adr/`, `docs/decisions/`, or `adr/` directory.
- Supersede or amend an existing ADR.

Do **not** use this skill for general technical writeups, runbooks, RFCs that are not decisions, or tutorials.

## What a good ADR is

- **Scoped to one decision.** If two decisions surface, write two ADRs and link them.
- **Written in the past or present tense of an accepted choice**, not as a proposal essay. The decision section states what *is* done.
- **Honest about trade-offs.** Every serious option gets pros and cons. If an option was dismissed without analysis, say so explicitly.
- **Immutable in spirit, mutable in practice.** Once accepted, don't rewrite history — supersede with a new ADR and link both ways.
- **Short.** Most ADRs fit on one screen. If it's getting long, that's usually two decisions.

## Workflow

Follow this order. Do not skip ahead.

### 1. Gather the decision

Before writing anything, confirm you have:

1. **The decision itself** — one sentence: "We will use X for Y."
2. **The driving forces** — what requirement, constraint, or problem forced a decision now.
3. **At least two considered options** — including the one chosen. A one-option ADR is a suspicious ADR.
4. **The rejection reasons** for each non-chosen option.
5. **Consequences** — both positive and negative follow-ons.

If any of these are missing, ask the user for them before drafting. Do not invent trade-offs.

### 2. Pick a template

Default to **MADR (full)**. Use others only on explicit signal:

| Template | Use when |
|---|---|
| MADR (full) | Default. Multiple options with pros/cons. See `references/template-madr-full.md`. |
| MADR (minimal) | User says "quick", "lightweight", or the decision has ≤2 options with one clearly dominant. See `references/template-madr-minimal.md`. |
| Nygard | User explicitly asks for "Nygard style", or the existing repo already uses it. See `references/template-nygard.md`. |
| Y-statement | User asks for a one-paragraph/one-liner decision, or for "sustainable architectural decisions" style. See `references/template-y-statement.md`. |
| Tyree & Akerman | Enterprise / regulated / heavy-governance contexts (explicit ask). See `references/template-tyree-akerman.md`. |

If the target repo already contains ADRs, **match the existing style** — inspect one file before drafting.

### 3. Name the file

Use the convention from Nygard's `adr-tools` and the joelparkerhenderson repo:

```
NNNN-present-tense-imperative-phrase.md
```

- `NNNN` = zero-padded 4-digit sequence (`0001`, `0002`, …).
- Kebab-case, lowercase, ASCII only.
- Imperative verb phrase, like a git commit subject: `use-postgresql`, `adopt-event-sourcing`, `switch-from-rest-to-grpc`.
- **Not** `adr-0001-…`, **not** the decision outcome (`postgresql.md`), **not** dated in the filename.

To pick the next number, run `scripts/next_id.py <adr-dir>`. It scans the directory and returns the next zero-padded ID.

### 4. Draft

Copy the chosen template from `references/` and fill it in. Rules:

- **Title line** matches the filename's verb phrase, Title Cased: `# Use PostgreSQL for primary storage`.
- **Status** is one of: `Proposed`, `Accepted`, `Rejected`, `Deprecated`, `Superseded by [NNNN](nnnn-…md)`. Default to `Proposed` unless the user says the decision is final.
- **Date** in ISO format (`YYYY-MM-DD`). Today's date by default.
- **Deciders** only if the user provides them. Don't invent names.
- **Context** describes the forces — technical, business, regulatory, team. Not a history lesson.
- **Considered options** — every option the team actually weighed, including "do nothing" if relevant.
- **Decision outcome** — the chosen option plus a one-paragraph justification.
- **Consequences** — split into positive, negative, and neutral/unknown. Be specific. "Increased complexity" is useless; "Requires us to run a second stateful service in production" is useful.
- **Links** — to related ADRs, tickets, RFCs, external references. Use relative paths for sibling ADRs.

### 5. Verify before presenting

Run through this checklist:

- [ ] Exactly one decision. If you can split it cleanly into two, do so.
- [ ] At least two considered options with real pros and cons.
- [ ] Rejection reasons are concrete, not vague ("too complex" without saying why).
- [ ] Consequences include at least one negative. Every decision has a cost.
- [ ] No marketing language. No "best-in-class", "cutting-edge", "industry-leading".
- [ ] File name matches convention.
- [ ] Status, date, and links are filled in.
- [ ] Total length is appropriate — roughly 150–600 words for a standard ADR. Significantly longer deserves scrutiny.

### 6. Output

Write the file. If an `adr/` or `docs/adr/` or `docs/decisions/` directory exists in the working tree, place it there. Otherwise ask where it should go, or default to `docs/adr/`.

If this ADR supersedes an earlier one, also update the earlier file's status to `Superseded by [NNNN](./nnnn-…md)` and add a reciprocal link. Don't delete the old ADR.

## Common mistakes to avoid

- **Writing a proposal, not a decision.** ADRs are records. Past-tense context, present-tense decision, future-tense consequences.
- **Only listing the winner's pros.** The losing options need real treatment, or the ADR is propaganda.
- **Burying the decision.** The "Decision" / "Decision Outcome" section should be findable in three seconds.
- **Omitting negative consequences.** If there are none, the decision probably wasn't architecturally significant.
- **Scope creep.** If a section starts describing a *second* decision, stop and open a second ADR.
- **Editing accepted ADRs in place.** Supersede instead.

## References

- `references/template-madr-full.md` — the default template.
- `references/template-madr-minimal.md` — stripped-down MADR.
- `references/template-nygard.md` — Michael Nygard's original format.
- `references/template-y-statement.md` — one-paragraph Y-statement.
- `references/template-tyree-akerman.md` — enterprise template.
- `references/examples.md` — worked examples of good vs. bad ADR prose.
- `scripts/next_id.py` — compute next zero-padded ADR ID for a directory.