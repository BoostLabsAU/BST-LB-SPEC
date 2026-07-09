# Requirements Template

> Requirements define what must be true.
> They do not prescribe implementation unless the implementation itself is a requirement.
>
> Requirements are stable. They should not be edited by the Coding LLM during implementation. If implementation exposes a missing requirement, **stop** and send it back to the Orchestrator.

Copy this template to `LLM/requirements/{area}.md`. `{area}` is a stable name (e.g. `auth`, `billing`, `lifecycle`), not a feature name.

---

# Requirements — {Area}

## Status
Draft / Approved / Superseded

Last updated: YYYY-MM-DD

## Goal
One paragraph. What this area must achieve.

## Users / actors
Who interacts with this area, and in what role.

## User outcomes
What the user can do, see, or rely on after this area is built.

## Functional requirements
- ...

## Non-functional requirements
Performance, availability, observability, etc. Only list what is binding.

## Security / trust requirements
- ...

## Data / privacy / residency requirements
- ...

## UX requirements
Only list constraints that affect product behavior. Visual polish belongs in design.

## Explicit non-goals
What this area does **not** cover. List enough to prevent scope creep.

## Acceptance criteria
Conditions under which this area can be declared "satisfied" at the requirement level. Phrased as observable facts, not test commands.

## Open questions
Unresolved decisions. Each entry should name an owner or be marked `unowned`.

## Links
- Related design docs:
- Related task lists:
