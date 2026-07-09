# Tasks

Task lists split approved authority into bounded work packages.

Use `LLM/TASKLIST_TEMPLATE.md`.

Rules:

- A work package (WP) is the unit of one focused coding session.
- WP IDs are globally unique across the project (one sequence for all task lists, e.g. WP-001, WP-002, …) and never renumbered.
- A WP is closed only when its audit verdict is Pass (`LLM/audits/WP-{ID}.md`).
- Per-WP closure truth lives in audits; task lists define scope and carry milestone/tranche/area status.
- Task-list status may be Draft, Approved, In Progress, Complete, or Blocked.
- Update task-list top-level status when the milestone/tranche/area state changes.
- If a WP stops or is blocked, the Orchestrator updates `LLM/CURRENT.md` and resolves/follows up before starting the next WP.
- If you already have an external task tracker, the task list here can be a thin pointer. The repo file remains the source of truth for scope and acceptance.
