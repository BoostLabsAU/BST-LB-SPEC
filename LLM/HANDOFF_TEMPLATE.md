# Coding LLM Handoff — [Feature Name]

## Context
You are adding/modifying [feature description] in the **[Project Name]** (`[project root path]`). [Stack info, e.g., PHP 8.2, Laravel 11, PSR-4 autoload].

[Brief explanation of what exists and what needs to change.]

## Read These Files First
1. `path/to/file1` — [why they need to read it]
2. `path/to/file2` — [why they need to read it]

⚠️ **Do NOT read files not listed above. Do NOT modify any other files besides those specified below.**

## Changes Required

### 1. `path/to/file` — [description]
[Detailed specification of what to add/change. Include:]
- Function signatures
- Logic flow
- Data schemas
- Expected output format
- Edge cases to handle

### 2. `path/to/another-file` — [description]
[Same level of detail]

## Rules
- [Module system rules, e.g., Use `require` / `module.exports` (CommonJS)]
- [Files NOT to touch]
- [Dependencies NOT to add]
- [Backward compatibility requirements]

## Verification
1. [Syntax check command] — no errors
2. [Feature test 1] — expected result
3. [Feature test 2] — expected result
4. [Regression test] — existing features still work

## Completion Report (REQUIRED)
Create `LLM/completions/[feature-name].md` with: What Was Changed, Verification Results, Files Modified.

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
