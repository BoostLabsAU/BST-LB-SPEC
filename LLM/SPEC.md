# Spec Index

`SPEC.md` indexes the project's BST-LB-SPEC execution structure. It is not the active work tracker.

Use `LLM/CURRENT.md` for live active state.

Distribution: `BST-LB-SPEC v2.0`. See the root README changelog for release history.

Keep this file short. Long content belongs in `requirements/`, `design/`, `tasks/`, or external authority docs.

## Update Policy

Update `SPEC.md` only when one of these changes:

- authority order or project mode
- existing authority docs
- role start prompts or context-loading policy files
- task-list inventory
- execution-layer structure

Do **not** update `SPEC.md` for every completion, audit, or active status change. Those belong in `LLM/CURRENT.md`, task-list files, completions, and audits.

## Authority Order

When two layers conflict, the higher layer wins.

1. **Requirements** — `LLM/requirements/` or external authority docs
2. **Design** — `LLM/design/` or external authority docs
3. **Tasks** — `LLM/tasks/`
4. **Handoffs** — `LLM/handoffs/`
5. **Completions** — `LLM/completions/` (evidence only)
6. **Audits** — `LLM/audits/` (verdict only)
7. **Rules** — `LLM/rules.md` (failure memory only)

Lower layers cannot override higher layers. `rules.md` cannot override requirements or design. Handoffs cannot override tasks. Completions cannot redefine scope. Audits can fail work, but cannot silently change requirements.

## Current Mode

Pick one. Update when the project's relationship to BST-LB-SPEC changes.

- [ ] **New Project Mode** — BST-LB-SPEC owns requirements/design/tasks. Use the templates in `LLM/`.
- [ ] **Existing Project Mode** — Authority docs live elsewhere in the repo. BST-LB-SPEC only adds the missing layers, typically tasks/handoffs/completions/audits.
- [ ] **Hybrid Mode** — Some layers come from external authority docs, others are authored under `LLM/`.

## Existing Authority Docs

Existing/Hybrid mode only. If your project has docs that already act as requirements or design, list them here so handoffs can cite them directly.

| Path | Acts as | Notes |
|---|---|---|
| `path/to/doc.md` | requirements / design / both | ... |

Delete this section if Mode is `New Project`.

## Start / Resume Prompts

| File | Purpose |
|---|---|
| `LLM/START_HERE.md` | Router that points to the role-specific start prompt. |
| `LLM/START_ORCHESTRATOR.md` | Fresh Orchestrator session entrypoint. |
| `LLM/START_CODER.md` | Fresh Coding LLM session entrypoint. |
| `LLM/START_AUDITOR.md` | Fresh Auditor session entrypoint. |
| `LLM/START_DEEP_PLANNER.md` | Fresh deep-planning / decomposition entrypoint. |
| `LLM/START_SCOUT.md` | One-time adoption/intake entrypoint for existing projects. |
| `LLM/ORCHESTRATOR_BOOTSTRAP.md` | Authoritative Orchestrator state machine; wins over older or root-level prompts. |
| `LLM/RESUME_PROMPT.md` | Pasteable resume prompt; points to the role-specific starts. |
| `LLM/context-loading.md` | Required/optional/not-default context policy by role. |
| `LLM/model-routing.md` | Optional cost/risk guidance for model lanes, testing-lane trials, and field notes. |

## Execution Templates

| File | Purpose |
|---|---|
| `LLM/REQUIREMENTS_TEMPLATE.md` | Template for requirements authored under `LLM/`. |
| `LLM/DESIGN_TEMPLATE.md` | Template for design authored under `LLM/`. |
| `LLM/TASKLIST_TEMPLATE.md` | Template for milestone/tranche/area task lists. |
| `LLM/HANDOFF_TEMPLATE.md` | Template for Orchestrator-to-Coder handoffs. |
| `LLM/COMPLETION_TEMPLATE.md` | Template for Coder completion reports and Orchestrator Return Prompt. |
| `LLM/AUDIT_TEMPLATE.md` | Template for audit verdicts. |

## Model Routing

Use `LLM/model-routing.md` when recommending a model/lane in handoffs, audit prompts, or deep-planning prompts. It ships model-agnostic; fill in its lane table during adoption (method and worked example: `MODEL_ROUTING_GUIDE.md` in the template repo).

Testing-lane field notes live in `LLM/model-notes/`.

## Requirements

New Project Mode: index requirement files here.

Existing Project Mode: external authority only by default.

Hybrid Mode: list only scoped `LLM/requirements/{feature}.md` bridges that are genuinely needed.

- `LLM/requirements/{area}.md` — one-line description

## Design

New Project Mode: index design files here.

Existing Project Mode: external authority only by default.

Hybrid Mode: list only scoped `LLM/design/{feature}.md` bridges that are genuinely needed.

- `LLM/design/{area}.md` — one-line description

## Task Lists

This section indexes task-list files only. Open the task-list file for milestone/tranche status, and use `LLM/CURRENT.md` for the active WP.

| Path | Purpose | Notes |
|---|---|---|
| `LLM/tasks/{area-or-milestone}.md` | one-line description | Status lives in the task-list file. |

## One-Time Intake Artifacts

The live drop-in `LLM/` layer does not keep one-time scout or preflight artifacts.

If a project needs intake mapping, run the one-time Scout session (`LLM/START_SCOUT.md`). Its durable output lands here (mode, Existing Authority Docs, Adoption Notes), in `LLM/design/baseline.md` when no architecture docs exist, and in task lists. Do not maintain scout output as a standing context layer.

## Adoption Notes

Use this section when first adopting BST-LB-SPEC on an existing project. List durable decisions only: what authority docs are external, what BST-LB-SPEC layers will be created, and any special project constraints.

- ...

## Current Active Work

See `LLM/CURRENT.md` for the active WP pointer and status. Do not duplicate that content here.
