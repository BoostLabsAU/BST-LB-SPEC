# Resume Prompt

Use this when starting a fresh chat window or recovering from context limits.

Pick the role first. Then use the matching start prompt.

---

You are working on this project's BST-LB-SPEC execution layer.

Treat this as a fresh start. Ignore prior chat memory and rehydrate from repository files only.

## Pick Role

Use exactly one:

- Orchestrator: read `LLM/START_ORCHESTRATOR.md` and follow it.
- Coding LLM: read `LLM/START_CODER.md` and follow it.
- Auditor: read `LLM/START_AUDITOR.md` and follow it.
- Deep planner: read `LLM/START_DEEP_PLANNER.md` and follow it.
- Scout (one-time adoption session): read `LLM/START_SCOUT.md` and follow it.

If no role is specified, ask one concise question for the role before proceeding. Do not default to Orchestrator when resuming a Coder, Auditor, or Deep Planner window.

## Universal Rules

- Project authority docs and/or `LLM/requirements/` + `LLM/design/` are authority.
- `LLM/tasks/` and `LLM/handoffs/` are execution instructions.
- `LLM/CURRENT.md` is the live active-work pointer.
- `LLM/SPEC.md` is the slow authority/index file, not a live tracker.
- `LLM/context-loading.md` defines required, optional, and not-default files by role.
- One-time scout/preflight files are not part of startup context.
- Do not load all authority docs or broad source context when the active handoff is enough.

## Output

Produce one short state summary:

- role
- current mode
- active WP
- active status
- next action

Then perform the requested role.

## Session Protocol Reminder

- Orchestrator session: persistent. Keep open across WPs. Resume here if context gets long.
- Coder session: fresh every handoff. Close after writing completion report and return prompt.
- Auditor session: fresh every audit. Close after writing verdict.
- The human copy-pastes prompts between windows. Coder and Auditor never share context.
