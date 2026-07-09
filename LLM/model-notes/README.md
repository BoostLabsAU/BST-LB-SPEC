# Model Notes

Field-trial notes from real BST-LB-SPEC work packages. An LLM reviewer reads these files to decide whether to update:

- project `LLM/model-routing.md`
- project use-case guidance, if the project has one
- broader benchmark/use-case docs, if the human asks

## Who Writes Notes

The **Orchestrator** writes notes after a WP closes when a **testing-lane** model was used or materially attempted the WP. Default-lane reliability observations normally stay in the audit verdict unless the human explicitly requests a field note or routing review.

The current testing lanes are whatever `LLM/model-routing.md` marks as `Testing`. Do not maintain a second list here.

## When To Write

| Situation | Write a note? |
|---|---|
| Testing model completed a WP | Yes - use `NOTE-TEMPLATE.md` |
| Testing model materially attempted a WP, then stopped/failed/follow-up was required | Yes - factual weaknesses only |
| Testing model did well | Yes + tell the human in chat |
| Human skipped testing model due to rush/quota/risk | Optional `skipped_trial` only; never a negative note against the model |
| Competent default used without trial intent | No note unless explicitly requested |

## Filename

`{model-slug}-{YYYY-MM-DD}-{wp-id-or-short-topic}.md`

Examples:

- `{model-slug}-2026-06-13-WP-022.md`
- `{model-slug}-2026-06-14-WP-004-css-polish.md`

## Review Guidance For LLM Readers

1. Prefer notes with `outcome: completed` and an independent audit path.
2. Weight `production_risk: high` notes more than low-risk trials.
3. Ignore `skipped_trial` notes for demotion decisions.
4. Promote a testing model only when multiple notes agree on the same `role_assigned` and `recommendation: promote_for_role`.
5. Require at least one medium- or high-risk note before promoting a model for production-sensitive work.
6. Do not treat a single successful low-risk note as enough to promote a model to high-risk default routing.
