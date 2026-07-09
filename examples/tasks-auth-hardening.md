# Task List - Auth Hardening

> Example file. In a real project this lives at `LLM/tasks/auth-hardening.md`.

## Status

In Progress

Last updated: 2026-07-01

## Source Requirements

- `docs/security-review-2026-06.md` (external authority — Existing Project Mode)

## Source Designs

- `docs/architecture.md#auth` (external authority)

## Goal

Close the two login-abuse findings from the June security review: unthrottled login attempts and missing lockout telemetry.

## Work Packages

### WP-001 - Rate-limit the login endpoint

- **Goal:** Reject repeated failed login attempts per IP+account with HTTP 429.
- **Authority refs:** `docs/security-review-2026-06.md#finding-1`, `docs/architecture.md#auth`
- **In-scope files or folders:** `src/routes/auth.js`, `src/middleware/rateLimit.js` (new), `test/auth.test.js`
- **Out-of-scope files or folders:** `src/models/`, `src/routes/` other than `auth.js`, any config or deployment files
- **Expected changes:** add a fixed-window limiter middleware; apply it to `POST /login` only; add tests for the 429 path.
- **Acceptance checks:** 6th failed attempt within 15 minutes returns 429 with `Retry-After`; successful logins are unaffected; existing auth tests still pass.
- **Migration impact:** none (in-memory store; Redis is a later WP).
- **Risk notes:** must not lock out users behind shared IPs on *successful* logins; count failures only.
- **Review focus:** off-by-one on the attempt threshold; limiter applied to the wrong routes.
- **Suggested harness/model:** bulk implementation lane per `LLM/model-routing.md`.
- **Suggested branch/worktree:** `wp-001`

### WP-002 - Lockout event logging

- **Goal:** Emit a structured `auth.lockout` event whenever the limiter blocks a request, for alerting.
- **Authority refs:** `docs/security-review-2026-06.md#finding-2`
- **In-scope files or folders:** `src/middleware/rateLimit.js`, `src/lib/logger.js`, `test/auth.test.js`
- **Out-of-scope files or folders:** alerting infrastructure (separate system)
- **Expected changes:** log event with account id hash, IP, window count; never log the password or raw account id.
- **Acceptance checks:** blocked request produces exactly one `auth.lockout` log line; no PII in the event.
- **Migration impact:** none.
- **Risk notes:** PII leakage in logs.
- **Review focus:** what exactly lands in the log payload.

## Rules For This Task List

- Add new WPs by appending. Do not renumber.
- A WP that turns out to be too large should be split rather than expanded.
- A WP is closed only when its audit passes in `LLM/audits/`.
- Per-WP closure truth lives in `LLM/audits/WP-{ID}.md`; do not add a duplicate per-WP status tracker here unless explicitly needed.
