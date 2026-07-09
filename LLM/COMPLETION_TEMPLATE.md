# Completion Template

Completion records what actually happened during one coding session.

Completion is written by the **Coder**, not the Auditor. The audit verdict goes in `LLM/audits/WP-{ID}.md`.

Completion does not redefine scope. If scope changed mid-session, list it under `Deviations From Handoff` and stop.

Copy this template to `LLM/completions/WP-{ID}.md`.

---

# Completion - WP-{ID} {Name}

## Status

Pass / Fail / Partial / Blocked / Stopped

Status meaning for completions:

- `Pass` - Coder believes the handoff is implemented and ready for audit.
- `Fail` - Coder attempted the handoff but could not produce a usable result, or verification failed in a way that makes the work not audit-ready.
- `Partial` - some work was completed, but scope remains unfinished.
- `Blocked` - external workspace state, missing decision, missing prerequisite, or handoff flaw prevents completion.
- `Stopped` - a handoff stop condition triggered before the Coder could safely continue.

This status is coder-reported execution outcome, not audit verdict. Only `LLM/audits/` can accept or reject work.

Completion `Pass` requires `Ready For Audit? Yes`. If the work is incomplete, blocked, stopped, or verification makes audit unsafe, use `Partial`, `Blocked`, `Stopped`, or `Fail` and set `Ready For Audit? No`.

Last updated: YYYY-MM-DD

## Handoff Followed

Path: `LLM/handoffs/WP-{ID}.md`

If this is a follow-up, also list:

- Follow-up handoff: `LLM/handoffs/WP-{ID}-followup-N.md`

## Verification Commands Run

| Command | Result | Notes |
|---|---|---|
| `<exact command>` | pass / fail / skipped | ... |

If a command was skipped or could not run, say why explicitly. Do not omit it.

## Files Changed

Exact list. Must match the handoff's `Expected changed files` or be explained under `Deviations From Handoff`.

- `path/to/file` - what changed
- `path/to/file` - what changed

If none, write `None`.

## Extra Files Explored

Files read outside `Read First` while implementing.

- `path/to/file` - reason

If none, write `None`.

## Skills Used (if any)

- `LLM/skills/{skill}.md` - helpful / confusing / partially helpful, with one-line reason

If none, write `None`.

## Deviations From Handoff

Anything implemented differently from the spec, with reason.

If none, write `None`.

## Open Items

Anything left unresolved that the auditor or follow-up handoff should know.

If none, write `None`.

## Risks / Concerns

Things you noticed that may bite later. Brief.

If none, write `None`.

## Commit / PR

- Branch: `wp-{id}` or `not a git worktree`
- Commit(s): `<sha>` or `none`
- PR: URL or `none yet`

## Ready For Audit?

Yes / No

If No, explain what blocks audit-readiness and what the Orchestrator should decide next.

## Orchestrator Return Prompt

Paste-ready prompt for the Orchestrator session. Always include this section, even when the coding session stops early, fails verification, is blocked, or is not ready for audit.

The human will copy this block and paste it into the Orchestrator window. After writing this completion report, **stop**. Do not start the next WP, audit your own work, or role-play the Orchestrator.

```markdown
The Coding LLM has completed/stopped WP-{ID}. Read the completion report at
`LLM/completions/WP-{ID}.md`.

Completion status: Pass / Fail / Partial / Blocked / Stopped
Ready For Audit? Yes / No

Summary:
- <what changed or what was attempted>
- <verification result or stop condition>
- <blocker or next required decision, if any>

If Completion status is Pass and Ready For Audit? is Yes, the Orchestrator should
set `LLM/CURRENT.md` to `awaiting-audit` and route to a fresh Auditor session with:

Read `LLM/START_AUDITOR.md` and follow it.

Audit WP-{ID}.

Audit inline only if the human explicitly opts into same-session/low-assurance
audit; if so, follow `LLM/START_AUDITOR.md` and `LLM/AUDIT_TEMPLATE.md`.

If not ready for audit, do not audit as complete. Decide whether to provide
missing workspace state, revise the task list, create a requirements/design bridge,
or write a follow-up handoff at `LLM/handoffs/WP-{ID}-followup-N.md`.
The Orchestrator must update `LLM/CURRENT.md` after that decision.
```
