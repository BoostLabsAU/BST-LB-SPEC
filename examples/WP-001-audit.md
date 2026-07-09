# Audit - WP-001 Rate-limit the login endpoint

> Example file. In a real project this lives at `LLM/audits/WP-001.md`, written by a fresh Auditor session.

## Verdict

Pass

Last updated: 2026-07-02

## Auditor Session

- Model / harness: project audit lane, fresh session
- Tools available: read+execute
- Assurance note: fresh auditor

## Checked Against

- Requirements: `docs/security-review-2026-06.md#finding-1`
- Design: `docs/architecture.md#auth`
- Task list entry: `LLM/tasks/auth-hardening.md#wp-001`
- Handoff: `LLM/handoffs/WP-001.md`
- Completion: `LLM/completions/WP-001.md`
- Diff / PR: branch `wp-001`, commit `9d81e02`

## Pre-Audit Gate

- Did the completion include `## Orchestrator Return Prompt`? Yes
- Did the completion status say `Pass`? Yes
- Did the completion say `Ready For Audit? Yes`? Yes

## Scope Review

- Did actual changed files match the handoff's `Expected changed files`? Yes — exactly the three listed files.

## Requirement Compliance

Finding-1 asks for login throttling per IP+account. Implemented as specified: failure-only counting, 6th failure in the window rejected. Satisfied.

## Design Compliance

Middleware pattern matches `docs/architecture.md#auth` conventions (factory function, applied per-route). Satisfied.

## Handoff Compliance

All three procedural changes implemented; stop conditions untriggered; one extra file explored and logged with reason. Compliant.

## Verification Review

- Were the handoff's verification commands actually run? Yes
- Were the results credible? Yes
- Did the auditor rerun any commands? Reran `npm test -- --grep "login"` — 9/9 pass. Additionally ran the full suite (`npm test`) — pass, no regressions elsewhere.

## Model / Lane Reliability

None

## Issues Found

- **Note** - `Retry-After` is computed from window start, so a client hammering at the window edge can see `Retry-After: 0` for one request. Harmless (the next request re-evaluates); flagging for the Redis follow-up WP.

## Required Follow-Up

None

## Merge / Close Decision

Allowed

## State Update Required

- `LLM/CURRENT.md`: move WP-001 to Recently Completed with this audit path and verdict Pass; populate `Next` with WP-002; proceed to the next handoff.
- `LLM/tasks/auth-hardening.md`: top-level status stays In Progress (WP-002 remains open).
- `LLM/SPEC.md`: no change.

## Notes

The in-memory reset risk noted in the completion is real but accepted in the task list (Redis is a later WP). No action needed now.
