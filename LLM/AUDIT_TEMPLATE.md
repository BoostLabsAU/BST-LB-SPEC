# Audit Template

Audit decides whether audit-ready work is accepted.

Audit is written by the **Auditor** or by the **Orchestrator acting as Auditor**, never by the Coder.

A standalone Auditor writes the verdict and required state-update instructions; the Orchestrator applies `LLM/CURRENT.md` transitions.

Fail audit = no merge/close. No documentation update may claim a WP is complete until verdict is Pass.

Copy this template to `LLM/audits/WP-{ID}.md`. If a follow-up audit is needed, append `-followup-N` to the WP-ID rather than overwriting.

Do not use this template for a completion that lacks `Status: Pass` or says `Ready For Audit? No`. In that case, send it to Orchestrator blocked/follow-up handling; the Orchestrator updates `LLM/CURRENT.md` and resolves the blocker first.

If the pre-audit gate fails because the return prompt is missing, do not write a normal audit verdict. Record/request the required Orchestrator state as `LLM/CURRENT.md` = `blocked; awaiting orchestrator decision` and require a corrected completion report.

---

# Audit - WP-{ID} {Name}

## Verdict

Pass / Fail / Follow-up required

Last updated: YYYY-MM-DD

## Auditor Session

- Model / harness:
- Tools available: read-only / read+execute
- Assurance note: fresh auditor / same-session low-assurance / other

## Checked Against

- Requirements: `LLM/requirements/{area}.md` sections X/Y, or external authority doc path
- Design: `LLM/design/{area}.md` sections X/Y, or external authority doc path
- Task list entry: `LLM/tasks/{area}.md#wp-{id}`
- Handoff: `LLM/handoffs/WP-{ID}.md`
- Completion: `LLM/completions/WP-{ID}.md`
- Diff / PR: `<URL or commit range>` or `not a git worktree`

## Pre-Audit Gate

- Did the completion include `## Orchestrator Return Prompt`? Yes / No
- Did the completion status say `Pass`? Yes / No
- Did the completion say `Ready For Audit? Yes`? Yes / No
- If No to any gate, stop the audit and instruct the Orchestrator to move the WP to blocked/follow-up handling instead of issuing a normal verdict.

## Scope Review

- Did actual changed files match the handoff's `Expected changed files`? Yes / No
- If No, list deltas and judge whether they are within scope.

## Requirement Compliance

Brief notes on whether each cited requirement is satisfied. Cite specific requirement sections, not whole docs.

## Design Compliance

Brief notes on whether the implementation matches the chosen approach. Cite specific design sections.

## Handoff Compliance

Did the Coder follow procedural steps, stop conditions, rules, completion-report requirements, and return-prompt requirements?

## Verification Review

- Were the handoff's verification commands actually run? Yes / No
- Were the results credible? Yes / No
- Did the auditor rerun any commands? List them and their outcomes.

## Model / Lane Reliability

If the WP used a testing lane, or a default lane still building trust on this project, include concise routing-relevant notes the Orchestrator can carry into model-routing review or a testing-lane field note: did the model match its assigned role, did it overclaim in the completion, were edge cases missed, would you assign it similar work again. If not relevant, write `None`.

## Issues Found

List each issue with severity:

- **Blocking** - audit fails because of this
- **Follow-up** - must be fixed before close, but can be handled by a follow-up handoff
- **Note** - informational; not blocking

## Required Follow-Up

If verdict is Fail or Follow-up required:

- Follow-up handoff path: `LLM/handoffs/WP-{ID}-followup-N.md`
- Status: branch/worktree remains active; no merge/close.
- Use `blocked; awaiting orchestrator decision` when the next step needs missing workspace state, a human decision, or authority/design clarification.
- Use `follow-up awaiting coder` when the remaining issue is in-scope and fixable by a follow-up handoff.

If verdict is Pass: write `None`.

On Pass, the Orchestrator must move the WP to `LLM/CURRENT.md#Recently Completed` with audit path and verdict, then set live status to `idle; awaiting orchestrator selection` or the next selected WP. Update task-list top-level status only if the milestone/tranche/area state changes. Do not add duplicate per-WP status unless the task list already uses that convention.

## Merge / Close Decision

Allowed / Blocked

## State Update Required

A standalone Auditor session that began with `LLM/START_AUDITOR.md` fills this section as instructions for the Orchestrator; do not edit `LLM/CURRENT.md`.

- `LLM/CURRENT.md`: describe required update after this audit.
  - Pass: move WP to Recently Completed, populate Next, and proceed to the next handoff unless the human pauses or no work remains.
  - Fail / Follow-up required: point to follow-up handoff or blocker state.
- `LLM/tasks/{area}.md`: update top-level milestone/tranche/area status only if it changed; update per-WP status only if that task list already uses per-WP status fields.
- `LLM/SPEC.md`: update only if task-list inventory, authority index, or execution structure changed.

## Notes

Anything the next session or future auditor should know. Brief.

---

The sections above are the audit artifact — everything up to here gets copied to `LLM/audits/WP-{ID}.md`. After writing the file, end the session with the paste-ready verdict summary defined in `LLM/START_AUDITOR.md#Final Response Shape`, then **stop**. Do not copy this instruction paragraph into the audit file.
