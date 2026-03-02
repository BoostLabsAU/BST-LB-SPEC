# Skills

Reusable, procedural instruction fragments for common task patterns in this project.

## What Is a Skill?

A Skill is a short markdown file (20–50 lines) that describes **how to perform a specific type of change** in this codebase. It captures the project's conventions, file locations, naming patterns, and verification steps for a repeatable task type.

Skills are **not handoffs** — they don't describe a specific feature. They describe a *pattern* that the Orchestrator can reference inside handoffs to avoid repeating the same instructions.

## Examples of Skills

- `add-api-endpoint.md` — Where to add routes, controller conventions, validation patterns
- `write-unit-tests.md` — Test file locations, naming conventions, assertion patterns
- `add-db-migration.md` — Migration file conventions, schema patterns, rollback requirements

## When to Create a Skill

The **Orchestrator** creates a skill when it notices itself writing similar handoff instructions for the **third time**. Two occurrences is a coincidence; three is a pattern.

Do NOT pre-create skills speculatively. Let them emerge from actual repeated work.

## How Skills Are Used

1. The Orchestrator writes a handoff as normal using `LLM/HANDOFF_TEMPLATE.md`
2. Instead of repeating common procedural steps, the handoff references a skill:
   ```
   ### 1. `routes/api.php` — Add new endpoint
   Follow the pattern in `LLM/skills/add-api-endpoint.md` with these specifics:
   - Route: POST /api/users/invite
   - Controller method: UserController@invite
   ```
3. The Coding LLM reads the skill file as part of "Read These Files First"
4. Prefer a small skill set per handoff (**0–3**). If you’d reference 4+ skills, inline only what’s needed or consolidate the Skills.

## Rules

- **Keep skills tiny.** 20–50 lines max. If it's longer, it's doing too much.
- **One pattern per file.** Don't combine "add endpoint" and "write tests" into one skill.
- **Skills describe patterns, not features.** They should be reusable across multiple handoffs.
- **Be selective.** Don’t attach skills to “easy” tasks where they add reading/compliance overhead.
- **Include at least one concrete example.** Give a minimal template or command sequence the Coding LLM can follow.
- **Don’t self-generate skills at execution time.** Never ask the Coding LLM to author skills mid-task; the Orchestrator curates skills after repeated patterns.
- **The Orchestrator owns skills.** Only the Orchestrator creates or updates them. The Coding LLM reads them.
- **Update, don't accumulate.** If a skill becomes outdated, update it. Don't create a v2 alongside v1.
