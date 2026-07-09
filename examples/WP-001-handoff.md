# Handoff - WP-001 Rate-limit the login endpoint

> Example file. In a real project this lives at `LLM/handoffs/WP-001.md`.

## Role

You are the Coding LLM for this work package.

You are not the product designer. You are not the architect. You must not redefine requirements. You must not broaden scope beyond the file lists below.

## Required Return Contract

Before your final chat response, write `LLM/completions/WP-001.md` using `LLM/COMPLETION_TEMPLATE.md`. The completion report must include `## Orchestrator Return Prompt`, and your final chat response must repeat the same paste-ready prompt. This applies to every outcome — complete, failed, partial, blocked, stopped, or skipped verification.

## Authority

- Requirements: `docs/security-review-2026-06.md#finding-1`
- Design: `docs/architecture.md#auth`
- Task list entry: `LLM/tasks/auth-hardening.md#wp-001`

If any of these conflict with this handoff, stop implementation, document the blocker in the completion report, set `Ready For Audit?` to `No`, and return control to the Orchestrator. Authority docs win.

## Model / Lane

Use the project's bulk implementation lane (see `LLM/model-routing.md`). Status: Default.
Reason: well-scoped single-endpoint change with exact tests.

## Git / Isolation

- **Branch / worktree:** `wp-001`
- **Base commit:** `4f2c9ab`
- **Expected changed files:** `src/routes/auth.js`, `src/middleware/rateLimit.js` (new), `test/auth.test.js`
- **Out-of-scope files (must not touch):** `src/models/`, other routes, config and deployment files
- **PR / commit target:** branch `wp-001`

## Read First

1. `src/routes/auth.js` - the endpoint being protected
2. `src/middleware/requestId.js` - the project's middleware conventions
3. `test/auth.test.js` - existing test patterns to extend

You may explore additional files only when blocked. Log every extra file and the reason in the completion report.

## Scope

Add a fixed-window rate limiter for failed login attempts and apply it to `POST /login` only. No other routes change; no persistence layer is added; lockout *logging* is WP-002, not this WP.

## Changes Required

### 1. `src/middleware/rateLimit.js` - new file

- Export `failedLoginLimiter({ windowMs = 15 * 60 * 1000, max = 5 })`.
- Key: `${ip}:${normalizedEmail}`; store counts in a `Map` with window expiry.
- Count only *failed* attempts (hook: call `limiter.recordFailure(key)` from the auth route on bad credentials).
- When count exceeds `max` within the window, respond `429` with a `Retry-After` header (seconds remaining in the window) and do not invoke the auth logic.
- Edge cases: malformed email still counts against IP; window reset clears the count; successful login does NOT increment.

### 2. `src/routes/auth.js` - apply limiter

- Apply `failedLoginLimiter()` to `POST /login` only.
- On failed credential check, record the failure before responding 401.

### 3. `test/auth.test.js` - tests

- 6th failed attempt within the window → 429 with `Retry-After`.
- 5 failures then a success → success passes, counter clears.
- Different account, same IP → independent counting per key.

## Stop Conditions

Stop and ask the Orchestrator if any of the following hold:

- the files actually present differ from `Read First` or `Expected changed files`
- implementation requires touching anything in `Out-of-scope`
- a dependency or new env var is needed but not listed
- verification cannot be attempted for any reason

## Rules

- Module/build system: CommonJS, Node 20.
- Dependencies NOT to add: no external rate-limit packages; in-memory only for this WP.
- Do not edit `LLM/requirements/`, `LLM/design/`, `LLM/tasks/`, `LLM/rules.md`, `LLM/SPEC.md`, `LLM/CURRENT.md`, or `LLM/skills/*.md`.
- After writing the completion report and return prompt, **stop**.

## Verification

1. `node --check src/middleware/rateLimit.js` - syntax OK
2. `npm test -- --grep "login"` - all login tests pass, including the three new ones

## Completion Report

Write your completion report to `LLM/completions/WP-001.md` using `LLM/COMPLETION_TEMPLATE.md`, including `## Orchestrator Return Prompt` for every outcome.
