# Completions

Completions are Coder-authored records of what actually happened during one coding session.

Use `LLM/COMPLETION_TEMPLATE.md`.

Rules:

- Completion is evidence and execution outcome, not audit acceptance.
- Completion status may be `Pass`, `Fail`, `Partial`, `Blocked`, or `Stopped`.
- Audit can proceed only when completion status is `Pass` and `Ready For Audit? Yes`.
- Every completion must include `## Orchestrator Return Prompt`.
- The Coder's final chat response must repeat the same paste-ready Orchestrator Return Prompt.
- If the return prompt is missing from a new completion, the coding session is procedurally incomplete.
- The Auditor must not edit the completion report.
- Skipped or non-runnable verification commands must still be listed with the reason.
