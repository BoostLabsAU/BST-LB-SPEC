# Coding LLM Handoff — [Feature Name]

## Context
You are adding/modifying [feature description] in the **[Project Name]** (`[project root path]`). [Stack info, e.g., PHP 8.2, Laravel 11, PSR-4 autoload].

[Brief explanation of what exists and what needs to change.]

## Read These Files First
1. `path/to/file1` — [why they need to read it]
2. `path/to/file2` — [why they need to read it]

> **Note to Orchestrator:** If applicable Skills exist in `LLM/skills/`, add up to **3** of the most relevant ones after item 2. If no skill applies (or this is a routine task), delete this note.

Start with the listed files; you may explore others if needed, but log every extra file and why in your completion report, and STOP to ask the user if the task scope expands. Do NOT modify files not listed in "Changes Required" below.

## Changes Required

### 1. `path/to/file` — [description]
[Provide Procedural Guidance: Give step-by-step instructions on how to implement this. Include:]
- Exact function signatures
- Step-by-step logic flow
- Data schemas
- Expected output format
- Specific edge cases to handle

### 2. `path/to/another-file` — [description]
[Same level of procedural detail]

## Rules
- [Module system rules, e.g., Use `require` / `module.exports` (CommonJS)]
- [Files NOT to touch]
- [Dependencies NOT to add]
- [Backward compatibility requirements]
- Do NOT edit `LLM/docs/RULES.md` or `LLM/skills/*.md` unless the handoff explicitly asks for it.

## Verification
Keep verification focused (1–3 commands) unless the handoff explicitly requires broader testing.
1. [Syntax check command] — no errors
2. [Feature test 1] — expected result
3. [Feature test 2] — expected result
4. [Regression test] — existing features still work

## Completion Report (REQUIRED)
Create `LLM/completions/[feature-name].md` containing:
- **Pass/Fail Status:** Did it pass all verification checks?
- **Verification Commands Run:** Exact commands you executed.
- **Extra Files Explored:** List any files you had to search outside the "Read These First" list and why.
- **Skills Used (if any):** List any `LLM/skills/*.md` you read and whether each was helpful or confusing.
- **What Was Changed:** Files modified and brief summaries.
- **Deviations from Handoff:** List anything you implemented differently from the spec and why. Write "None" if you followed the spec exactly.
- **Metrics:** (Optional) Time taken or Token/Cost usage, if available in your interface.

## Final Output to User (REQUIRED)
End your response with:

```
---
## ✅ Implementation Complete

**Summary:** [1-2 sentence summary]

**Files changed:**
- `path/to/file` — description
- ...

**Completion report written to:** `LLM/completions/[feature-name].md`

---

### 🔁 Next Step — Paste this into your Orchestrator:

> The coding LLM has finished the **[Feature Name]** implementation.
> Read the completion report at `LLM/completions/[feature-name].md`.
> Perform a code review on the modified files against the handoff spec at `LLM/HANDOFF_[FEATURE].md` and run syntax checks.
> If there are deviations or issues, provide a follow-up prompt for the Coding LLM to fix them.
> If the review passes, update the project documentation and ask me what I'd like to work on next.
```
