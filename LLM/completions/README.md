# Completion Reports

This directory holds completion reports written by the Coding LLM after each implementation.

Each file documents what was changed, verification results, and files modified.

## Naming Convention
`{feature-name}.md` — matches the handoff filename

## Purpose
- Provides structured output for the Orchestrator to audit
- Creates an audit trail of all implementations
- Documents verification steps that were run
- Captures whether any referenced `LLM/skills/*.md` were helpful or confusing (so the Orchestrator can prune/repair skills)
