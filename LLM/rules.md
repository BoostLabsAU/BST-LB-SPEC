# Rules

`rules.md` is **failure memory, not architecture memory.**

Rules are added only after a repeated AI failure or a high-risk mistake. They are operational, not product architecture.

Rules **cannot override** requirements, design, or task scope. Higher layers win.

## How To Use This File

- Add a rule when the same model failure recurs, or once when the cost of the failure is high.
- Keep each rule to one line where possible.
- Prune stale rules. A rule older than about 3 months without a recurrence should be reviewed for removal.
- Do not put architecture, conventions, or project-specific facts here. Those belong in `LLM/design/` or in your project's existing authority docs.
- Do not restate workflow procedure here. The role start prompts, `LLM/ORCHESTRATOR_BOOTSTRAP.md`, and the templates already carry it.

## Workflow Invariants

These three are restated from the role prompts because breaking them silently corrupts the workflow. They are exempt from pruning.

- Audit failure blocks merge/close. No documentation update may claim completion until the audit verdict is Pass.
- The Coder never edits `LLM/CURRENT.md`, `LLM/SPEC.md`, requirements, design, task lists, rules, or skills unless the active handoff explicitly authorizes it.
- Every completion report and final Coder response must include `## Orchestrator Return Prompt`, for every outcome.

## Project Rules

Add your own recurring-failure rules below. Keep this section tiny.
