# Design Template

> Design explains **how** approved requirements will be satisfied.
>
> Design cannot invent new requirements. If design reveals a missing requirement, update requirements first, then return to design.
>
> Design should be specific enough to create tasks, but it is not a completion report.

Copy this template to `LLM/design/{area}.md`. `{area}` should match the requirements doc it satisfies.

---

# Design — {Area}

## Status
Draft / Approved / Superseded

Last updated: YYYY-MM-DD

## Requirements satisfied
List the requirements this design addresses, by ID or section reference.

- `LLM/requirements/{area}.md` — sections X, Y

## Chosen approach
One paragraph stating the approach in plain language. The "what we are doing and why" answer.

## Architecture
Components, boundaries, and how they interact. Diagrams allowed but not required.

## Data model
Tables, fields, types, invariants. Reference the canonical schema location if one exists.

## Interfaces / APIs
Public interfaces (HTTP, RPC, CLI, library) that other code depends on.

## State transitions
For anything stateful: states, allowed transitions, terminal states, recovery.

## Error handling
Failure modes, retries, idempotency, fallbacks. Be explicit when fail-closed vs fail-open.

## Security model
Trust boundaries, authentication, authorization, secret handling.

## Migration / compatibility notes
What existing data, code, or contracts this affects, and how to roll forward.

## Alternatives considered
Brief list of other approaches and why they were rejected.

## Risks
Known unknowns, sharp edges, and what would invalidate this design.

## Links
- Related task lists:
