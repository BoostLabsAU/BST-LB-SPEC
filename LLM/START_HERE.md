# Start Here

This folder is the project's live BST-LB-SPEC execution layer. It controls planning, handoff, completion, audit, and context loading. It does not replace project authority docs.

## Pick The Correct Start Prompt

Use exactly one role entrypoint for a fresh session:

| Role | Start prompt |
|---|---|
| Orchestrator | `LLM/START_ORCHESTRATOR.md` |
| Coding LLM | `LLM/START_CODER.md` |
| Auditor | `LLM/START_AUDITOR.md` |
| Deep planner / decomposer | `LLM/START_DEEP_PLANNER.md` |
| Scout (one-time, adoption) | `LLM/START_SCOUT.md` |

Do not use this file as a combined role prompt. It is only a router.

## Live State Rule

`LLM/CURRENT.md` is the only live active-work pointer.

- If it names an active work package, continue that state.
- If it says a WP is blocked, do not start a new WP until the Orchestrator resolves the blocker.
- If it is contradictory, ask one concise blocking question.

`LLM/SPEC.md` is a slow-moving authority/index file. It is not the active tracker.

## Project Mode

Record project mode in `LLM/SPEC.md`:

- New Project Mode: BST-LB-SPEC owns requirements/design/tasks.
- Existing Project Mode: project authority docs live elsewhere; BST-LB-SPEC adds execution layers.
- Hybrid Mode: some layers are external and some are authored under `LLM/`.

## One-Time Intake Artifacts

The drop-in `LLM/` layer does not keep one-time scout/preflight artifacts. If a project needs intake mapping, run the one-time Scout session (`LLM/START_SCOUT.md`); durable decisions land in `LLM/SPEC.md`, `LLM/design/baseline.md` when no architecture docs exist, and task lists.
