# Completion - WP-001 Rate-limit the login endpoint

> Example file. In a real project this lives at `LLM/completions/WP-001.md`, written by the Coder.

## Status

Pass

This status is coder-reported execution outcome, not audit verdict. Only `LLM/audits/` can accept or reject work.

Last updated: 2026-07-02

## Handoff Followed

Path: `LLM/handoffs/WP-001.md`

## Verification Commands Run

| Command | Result | Notes |
|---|---|---|
| `node --check src/middleware/rateLimit.js` | pass | |
| `npm test -- --grep "login"` | pass | 9 tests: 6 existing + 3 new, all green |

## Files Changed

- `src/middleware/rateLimit.js` - new; fixed-window limiter keyed by `${ip}:${normalizedEmail}`, failure-only counting, 429 + `Retry-After`
- `src/routes/auth.js` - limiter applied to `POST /login`; failure recorded before 401
- `test/auth.test.js` - three new tests per handoff

## Extra Files Explored

- `src/lib/email.js` - reason: confirmed the existing `normalizeEmail()` helper before writing key derivation

## Skills Used (if any)

None

## Deviations From Handoff

None

## Open Items

None

## Risks / Concerns

- In-memory `Map` store resets on process restart, so a restart clears counters. Accepted per handoff scope (Redis is a later WP), noting it for the auditor.

## Commit / PR

- Branch: `wp-001`
- Commit(s): `9d81e02`
- PR: none yet

## Ready For Audit?

Yes

## Orchestrator Return Prompt

```markdown
The Coding LLM has completed WP-001. Read the completion report at
`LLM/completions/WP-001.md`.

Completion status: Pass
Ready For Audit? Yes

Summary:
- Added failure-only fixed-window rate limiting to POST /login (429 + Retry-After); new middleware `src/middleware/rateLimit.js`.
- Verification: syntax check pass; `npm test -- --grep "login"` pass (9/9, including 3 new tests).
- No blockers. One noted risk: in-memory counters reset on process restart (accepted in handoff scope).

The Orchestrator should set `LLM/CURRENT.md` to `awaiting-audit` and route to a
fresh Auditor session with:

Read `LLM/START_AUDITOR.md` and follow it.

Audit WP-001.
```
