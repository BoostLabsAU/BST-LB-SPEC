# Context Loading Policy

Status: Active startup and context-loading policy for BST-LB-SPEC sessions.

Project authority docs remain the source of product, architecture, and design truth. `LLM/` remains execution control. Load the smallest context set that can safely perform the current role.

Do not read or print `.env`, credentials, private keys, token files, archives, dependency folders, build output, or unrelated source files.

## Role Entrypoints

Use one role-specific start prompt:

- Orchestrator: `LLM/START_ORCHESTRATOR.md`
- Coder: `LLM/START_CODER.md`
- Auditor: `LLM/START_AUDITOR.md`
- Deep planner: `LLM/START_DEEP_PLANNER.md`
- Scout (one-time, adoption): `LLM/START_SCOUT.md`

`LLM/START_HERE.md` is only a router to these prompts.

## Live State Versus Slow Index

- `LLM/CURRENT.md` is the volatile live active-work pointer. The Orchestrator updates it after every status change.
- `LLM/SPEC.md` is a slow-moving authority/index file. Update it only when the authority set, project mode, task-list inventory, role start prompts, or execution structure changes.

Do not use `SPEC.md` as a current-work tracker.

## Allowed Current Status Values

Use these status strings in `LLM/CURRENT.md`:

- `idle; awaiting orchestrator selection`
- `awaiting-coder`
- `awaiting-completion`
- `awaiting-audit`
- `blocked; awaiting orchestrator decision`
- `follow-up awaiting coder`
- `complete`

If none fits, use `blocked; awaiting orchestrator decision` and explain the blocker.

Use `complete` only when the human explicitly pauses or ends the pipeline with no next WP planned. Do not use it automatically at tranche boundaries.

## Orchestrator Sessions

Minimum startup files:

1. `LLM/CURRENT.md`
2. `LLM/ORCHESTRATOR_BOOTSTRAP.md`

Load on demand:

- `LLM/SPEC.md` when checking authority/index structure, selecting a new task list, or planning/auditing against authority.
- `LLM/rules.md` when writing handoffs, processing completions, auditing, or investigating repeated process failures.
- `LLM/context-loading.md` before widening context beyond the active state.
- `LLM/model-routing.md` only when preparing a handoff, audit prompt, or deep-planning prompt that needs a model/lane recommendation.
- `LLM/model-notes/` only when reviewing testing-lane evidence or writing a required field-trial note.

Then load only what the active state requires. The per-state actions and context are defined once, in `LLM/ORCHESTRATOR_BOOTSTRAP.md#State Machine` — follow that file rather than a summary here. As context guidance: each state needs only the artifacts it names (the active handoff, the active completion, the audit inputs); no state requires broad planning context except the `idle` selection flow, which additionally reads the task list.

Not loaded by default:

- all authority docs
- broad source trees
- unrelated skills
- one-time scout/preflight artifacts

## Coder Sessions

Required task context:

1. the active handoff named by the user or by `LLM/CURRENT.md`
2. the handoff's `Read First` files
3. exact files needed to implement or verify the handoff
4. `LLM/COMPLETION_TEMPLATE.md` before writing the completion report

Optional:

- additional files found by reference search, only when needed to implement or verify the handoff
- `LLM/skills` files only when the handoff explicitly names them

Not loaded by default:

- broad authority docs
- unrelated task lists
- broad source directories
- unrelated skills
- one-time scout/preflight files

Coder sessions must record any extra files explored in the completion report.

Coder sessions do not update `LLM/CURRENT.md`; the Orchestrator owns live-state transitions.

## State Transition Ownership

- Orchestrator updates `LLM/CURRENT.md` from `awaiting-coder` or `follow-up awaiting coder` to `awaiting-completion` when it issues the coder prompt.
- Coder writes or updates only the completion report and final response.
- Orchestrator updates `LLM/CURRENT.md` to `awaiting-audit`, `blocked; awaiting orchestrator decision`, or `follow-up awaiting coder` after reading the completion.
- After an audit verdict, the Orchestrator updates `LLM/CURRENT.md`: Pass moves the WP to `CURRENT.md#Recently Completed` and returns to idle/next WP; Fail or Follow-up moves to follow-up/blocker state.
- A standalone Auditor records the required `CURRENT.md` update in the audit report and final response, but does not edit live state.
- A standalone Deep Planner states the required Orchestrator state update; only an Orchestrator acting as Deep Planner in the same session applies it.
- Per-WP closure truth lives in `LLM/audits/WP-{ID}.md`; task lists carry scope and milestone/tranche-level status, not a duplicate per-WP tracker by default. If a task list already has per-WP status fields, update them after audit.

## Required Coder Return Contract

Every coder completion report must include `## Orchestrator Return Prompt`.

Every final coder chat response must include the same paste-ready prompt.

This applies to every outcome: Pass, Fail, Partial, Blocked, Stopped, skipped verification, or handoff stop condition.

A coding session that omits the Orchestrator Return Prompt is procedurally incomplete even if files changed.

## Auditor Sessions

Required audit context:

1. `LLM/handoffs/WP-{ID}.md`
2. `LLM/completions/WP-{ID}.md`
3. the relevant `LLM/tasks/` entry
4. authority docs cited by the handoff or task
5. changed-file or diff evidence available in the workspace
6. the handoff's verification commands, rerun when possible
7. `LLM/AUDIT_TEMPLATE.md`

If the completion says `Ready For Audit? No`, do not write a normal pass/fail audit. Report that the WP is blocked and requires Orchestrator resolution or a follow-up handoff.

Not loaded by default:

- all authority docs beyond cited authority
- unrelated source directories
- skills unless the handoff cited them

## Deep-Planning Sessions

Use deep planning only when the next unit of work cannot be safely handed to a coder from current authority docs, task lists, and handoff patterns.

Required context:

- `LLM/SPEC.md`
- `LLM/CURRENT.md`
- `LLM/context-loading.md`
- relevant project authority docs
- relevant `LLM/requirements/`, `LLM/design/`, and `LLM/tasks/` files

Optional:

- targeted source files only when planning depends on verified implementation shape
- skills only as reference context when directly relevant and explicitly named in the resulting handoff

Not loaded by default:

- all source files
- all skills
- archives, dependency folders, build output, or secrets

## Scout Sessions (One-Time, Adoption)

The Scout (`LLM/START_SCOUT.md`) is the deliberate exception to minimal context: it may read the repository broadly to map stack, structure, verification commands, and existing authority docs. The secrets policy above still applies in full.

Scope limits:

- One session per adoption; close it after outputs are approved.
- Output is capped (~200-line baseline, ≤15 anchors) even though reading is broad.
- Durable output lands only in `LLM/SPEC.md`, `LLM/design/baseline.md` (only when no usable architecture docs exist), and future task lists. No `LLM/scout/` folder, no standing scout artifacts.

## Requirements And Design Bridges

`LLM/requirements/` and `LLM/design/` may be first-class authority in New Project Mode.

In Existing Project Mode, create scoped `LLM/requirements/{feature}.md` or `LLM/design/{feature}.md` only when a large or ambiguous feature needs a feature-local bridge before tasking.

Bridge files must cite external authority and remain subordinate to it. They are not default layers for normal work packages.

## Skills Reference Context

`LLM/skills/` is for reusable patterns that repeat across handoffs.

Use skills only when an active handoff names the exact skill file. Skills cannot broaden scope, override authority, or replace task-list acceptance checks.
