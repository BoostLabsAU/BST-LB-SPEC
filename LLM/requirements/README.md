# Requirements

Approved product/system truth. One file per area, named `{area}.md` (e.g. `auth.md`, `billing.md`).

Use `LLM/REQUIREMENTS_TEMPLATE.md` for the structure.

Rules:
- Requirements are stable. The Coding LLM must not edit them.
- Requirements do not prescribe implementation unless the implementation itself is a requirement.
- If you already have authoritative requirement docs elsewhere in the repo, **do not duplicate them here**. Reference them from `LLM/SPEC.md` instead.
