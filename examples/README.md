# Examples — One Complete WP Lifecycle

A filled-in example of one work package travelling the whole chain, so you can see what good artifacts look like before writing your own. The project is a small Express API; the WP adds rate limiting to a login endpoint.

Read them in order:

| # | File | Written by | Stage |
|---|---|---|---|
| 1 | `tasks-auth-hardening.md` | Orchestrator | Task list with two WPs (`LLM/tasks/` in a real project) |
| 2 | `WP-001-handoff.md` | Orchestrator | Executable spec for one coding session (`LLM/handoffs/WP-001.md`) |
| 3 | `WP-001-completion.md` | Coder | Evidence of what actually happened (`LLM/completions/WP-001.md`) |
| 4 | `WP-001-audit.md` | Auditor (fresh session) | Verdict (`LLM/audits/WP-001.md`) |

These files are illustrations — do not copy them into your project's `LLM/` folder. Use the templates in `LLM/` and let your own artifacts accumulate.

## What `LLM/CURRENT.md` said along the way

| Moment | Status |
|---|---|
| Orchestrator wrote the handoff, prompt not yet issued | `awaiting-coder` |
| Orchestrator issued the Coder prompt | `awaiting-completion` |
| Completion arrived: `Status: Pass`, `Ready For Audit? Yes` | `awaiting-audit` |
| Audit verdict Pass; WP moved to Recently Completed; WP-002 queued in `#Next` | `idle; awaiting orchestrator selection` → immediately `awaiting-coder` for WP-002 |

If the completion had come back `Blocked`, `Partial`, `Stopped`, or `Ready For Audit? No`, the status would instead move to `blocked; awaiting orchestrator decision`, and after a follow-up handoff to `follow-up awaiting coder` — never to `awaiting-audit`.
