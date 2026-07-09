# Handoffs

Handoffs are Orchestrator-authored executable specs for Coding LLM sessions.

One file per work package, named `WP-{ID}.md`. Follow-ups append `-followup-N`.

Use `LLM/HANDOFF_TEMPLATE.md`.

Rules:

- One active handoff or follow-up at a time unless the user explicitly changes workflow.
- A handoff cannot redefine requirements, design, or task scope.
- `Read First` should contain no more than 3 priority files.
- Every handoff must include the Required Return Contract.
- The Coder must write a completion report and include `## Orchestrator Return Prompt` in both the report and final chat response.
- `LLM/CURRENT.md` must point at the active handoff/follow-up and status.
- Keep old handoffs for resume context and audit history.
