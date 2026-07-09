# Audits

Audits are verdict files for audit-ready work packages. They may be written by an Auditor or by an Orchestrator acting as Auditor.

One file per WP, named `WP-{ID}.md`. Follow-ups append `-followup-N`.

Use `LLM/AUDIT_TEMPLATE.md`.

Rules:

- Written by an Auditor or by an Orchestrator acting as Auditor, never by the Coder.
- Completion is evidence, not acceptance.
- Do not audit a completion as complete unless it has `Status: Pass` and `Ready For Audit? Yes`.
- **Fail audit = no merge/close.** No documentation update may claim completion until verdict is Pass.
- Recommended: audit in a different session/role from the coder. If one session role-plays both, mark the audit low-assurance.
- After audit, the Orchestrator updates `LLM/CURRENT.md`; a standalone Auditor records the required update instead of editing live state.
