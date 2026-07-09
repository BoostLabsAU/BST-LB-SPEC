# Task List Template

Task lists split approved authority into bounded work packages.

One WP fits one focused coding session. Do not combine unrelated subsystems in one WP.

A task list may cover a domain, milestone, tranche, or feature — whichever boundary makes sense.

**WP IDs:** zero-padded, assigned by the Orchestrator in creation order, **globally unique across the whole project** (WP-001, WP-002, …) — one sequence shared by every task list, never restarted per list and never renumbered. Handoffs, completions, and audits are all filed under the WP ID, so a duplicate ID collides on disk. Follow-ups append `-followup-N` to the existing ID.

Copy this template to `LLM/tasks/{area-or-milestone}.md`.

---

# Task List - {Area / Milestone / Tranche}

## Status

Draft / Approved / In Progress / Complete / Blocked

Last updated: YYYY-MM-DD

Status notes, if needed.

## Source Requirements

- `LLM/requirements/{area}.md` or external authority docs

## Source Designs

- `LLM/design/{area}.md` or external authority docs

## Goal

One paragraph stating what this milestone/tranche delivers.

## Work Packages

### WP-{ID} - {Name}

- **Goal:** one sentence.
- **Authority refs:** no more than 3 paths or sections this WP depends on.
- **In-scope files or folders:** exact list. No globs unless globs are unambiguous.
- **Out-of-scope files or folders:** exact list. The Coder must stop and ask before touching anything outside in-scope.
- **Expected changes:** procedural, ordered.
- **Acceptance checks:** observable facts, not just test commands.
- **Migration impact:** schema, config, deployment.
- **Risk notes:** what could go wrong.
- **Review focus:** what the auditor should pay attention to.
- **Suggested harness/model:** optional.
- **Suggested branch/worktree:** optional.

### WP-{ID} - {Name}

Repeat as needed.

## Rules For This Task List

- Add new WPs by appending. Do not renumber.
- A WP that turns out to be too large should be split rather than expanded.
- A WP is closed only when its audit passes in `LLM/audits/`.
- Per-WP closure truth lives in `LLM/audits/WP-{ID}.md`; do not add a duplicate per-WP status tracker here unless explicitly needed.
- Update this task list's top-level status when the milestone/tranche/area moves Draft -> Approved -> In Progress -> Complete / Blocked.
- If a WP stops or is blocked, the Orchestrator updates `LLM/CURRENT.md` and writes a follow-up handoff or task-list correction before starting a new WP.
