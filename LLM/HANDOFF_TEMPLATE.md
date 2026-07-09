# Handoff Template

A handoff is the executable instruction for **one** coding session.

The Coding LLM must not redefine requirements, design, or task scope. If the handoff conflicts with any higher layer, the Coder must stop and ask the Orchestrator.

Copy this template to `LLM/handoffs/WP-{ID}.md`. The filename should match the work-package ID from `LLM/tasks/`.

---

# Handoff - WP-{ID} {Name}

## Role

You are the Coding LLM for this work package.

You are not the product designer.
You are not the architect.
You must not redefine requirements.
You must not broaden scope beyond the file lists below.

## Required Return Contract

You must always return control to the Orchestrator.

Before your final chat response, write `LLM/completions/WP-{ID}.md` using `LLM/COMPLETION_TEMPLATE.md`.

That completion report must include `## Orchestrator Return Prompt`.

Your final chat response must include the same paste-ready Orchestrator Return Prompt.

This applies to every outcome:

- implementation complete
- implementation failed
- partial implementation
- skipped or failed verification
- blocked external prerequisite
- handoff stop condition
- not ready for audit

Do not end the coding session without the Orchestrator Return Prompt.

## Authority

- Requirements: `LLM/requirements/{area}.md` sections X/Y, or external authority doc path
- Design: `LLM/design/{area}.md` sections X/Y, or external authority doc path
- Task list entry: `LLM/tasks/{area}.md#wp-{id}`

If any of these conflict with this handoff, stop implementation, document the blocker in the completion report, set `Ready For Audit?` to `No`, and return control to the Orchestrator. Authority docs win.

## Model / Lane

Name the coder model and whether it is **Default** or **Testing**, per the project's lane table in `LLM/model-routing.md` (routing guidance, not authority).

Example (default):

```text
Use {model}. Status: Default.
Reason: {task shape this lane covers, e.g. bulk implementation with strict handoff}.
```

Example (testing):

```text
Use {testing-model}. Status: Testing.
Reason: {why this trial — role match / human request / quota fallback}.
Competent fallback: {default-model}.
Field note required after WP close: LLM/model-notes/ (see model-routing.md).
```

Do not assign a testing lane to high-risk production work without naming the competent fallback and keeping independent audit on a competent auditor lane.

For Testing lanes, state that the Orchestrator must write the field note after the WP closes.

## Git / Isolation

- **Branch / worktree:** `wp-{id}` suggested
- **Base commit:** `<sha>` of the branch point, or `not a git worktree`
- **Expected changed files:** exact list
- **Out-of-scope files (must not touch):** exact list copied from the WP
- **PR / commit target:** branch name or PR URL

## Read First

List no more than 3 priority files. Resist the urge to add more.

1. `path/to/file1` - why
2. `path/to/file2` - why
3. `path/to/file3` - why

You may explore additional files only when blocked. Log every extra file and the reason in the completion report.

## Skills / Reference Context

Use this section only when reusable patterns or implementation references are directly relevant. These files are reference context, not authority.

- Skill: `LLM/skills/{skill}.md` - why, optional
- Source/reference: `path/to/file` - why, optional

Do not load unrelated skills. Skills/reference context cannot broaden scope or override authority. Skill files listed here do not count against the 3-file `Read First` cap.

## Scope

One paragraph stating what changes in this session and what does not. Repeat the in-scope/out-of-scope lists if they are short.

## Changes Required

Procedural, per-file, ordered. For each file:

### 1. `path/to/file` - {summary}

- exact function signatures or contract
- step-by-step logic
- data shape
- expected output / behavior
- specific edge cases

### 2. `path/to/file` - {summary}

Repeat as needed.

## Stop Conditions

Stop and ask the Orchestrator if any of the following hold:

- the files actually present differ from `Read First` or `Expected changed files`
- implementation requires touching anything in `Out-of-scope`
- implementation contradicts requirements or design
- a migration, dependency, or new env var is needed but not listed
- verification cannot be attempted for any reason
- the expected-changed-file list is wrong or incomplete
- the WP ID or completion status needed for the return prompt is unknown

## Rules

- Module/build system: ...
- Files NOT to touch in addition to Out-of-scope: ...
- Dependencies NOT to add: ...
- Backward-compatibility constraints: ...
- Do not edit `LLM/requirements/`, `LLM/design/`, `LLM/tasks/`, `LLM/rules.md`, `LLM/SPEC.md`, `LLM/CURRENT.md`, or `LLM/skills/*.md` unless this handoff explicitly authorizes it.
- By default, the Orchestrator owns `LLM/CURRENT.md` transitions. The Coder writes the completion report and return prompt only.
- After writing the completion report and return prompt, **stop**. Do not start the next WP, audit your own work, or role-play the Orchestrator. The human copies your return prompt into the Orchestrator window, and the Orchestrator decides what happens next.

## Verification

1-3 focused commands. Prefer real evidence over breadth.

1. `<command>` - expected outcome
2. `<command>` - expected outcome
3. `<command>` - expected outcome

## Completion Report

Write your completion report to `LLM/completions/WP-{ID}.md` using `LLM/COMPLETION_TEMPLATE.md`.

The completion report must include `## Orchestrator Return Prompt`: a paste-ready prompt back to the Orchestrator that summarizes status, verification result, blockers, audit-readiness, and next decision.

Include it for every outcome, including `Pass`, `Fail`, `Partial`, `Blocked`, `Stopped`, skipped verification, or a handoff stop condition.

## Final Response Required

End every response that closes or stops the coding session with this exact block. The human will copy the `### Next step` section and paste it into the Orchestrator window.

```markdown
## Implementation Complete / Failed / Stopped - WP-{ID}

**Summary:** <1-2 sentences>

**Files changed:**
- path/to/file - what changed
- ...

**Completion report:** `LLM/completions/WP-{ID}.md`

**Ready For Audit?** Yes / No (if No, explain)

---

### Next step - paste this into the Orchestrator session:

<copy the `## Orchestrator Return Prompt` body from the completion report here>
```
