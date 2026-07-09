# Current Work

Tiny live pointer. Not a full project tracker.

The Orchestrator updates `CURRENT.md` after every workflow status change. `LLM/SPEC.md` is not the active tracker.

One active WP at a time. If this file names multiple active WPs or a new WP is selected while another is non-idle, set status to `blocked; awaiting orchestrator decision` until the human or Orchestrator resolves the conflict.

## Allowed Status Values

- `idle; awaiting orchestrator selection`
- `awaiting-coder`
- `awaiting-completion`
- `awaiting-audit`
- `blocked; awaiting orchestrator decision`
- `follow-up awaiting coder`
- `complete`

`complete` means the human explicitly paused or ended the pipeline with no next WP planned. Do not use it automatically at tranche boundaries.

## Active

Keep the active file pointers exact so the Orchestrator does not have to hunt.

- **Task list:** `LLM/tasks/{area-or-milestone}.md` or `None`
- **Work package:** `WP-{ID} {Name}` or `None`
- **Handoff:** `LLM/handoffs/WP-{ID}.md` or `None`
- **Worktree / branch:** `None`
- **Status:** `idle; awaiting orchestrator selection`
- **Completion:** `LLM/completions/WP-{ID}.md` or `None`
- **Audit:** `LLM/audits/WP-{ID}.md` or `None`

## Blocked

WPs that started but cannot progress. Include reason and next required Orchestrator decision.

- None.

## Next

One or two WPs queued up. Do not pre-issue an entire milestone/tranche here.

After a Pass audit, the Orchestrator should pre-fill this section with the next open WP from the task list so the next session can proceed immediately.

- None.

## Recently Completed

Last few WPs with audit Pass. Keep short.

- None.
