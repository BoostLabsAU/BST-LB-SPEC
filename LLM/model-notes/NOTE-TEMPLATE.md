# Field Trial Note Template

Copy this file to `{model-slug}-{YYYY-MM-DD}-{wp-id-or-topic}.md` when a testing-lane model completes, materially attempts, or is explicitly skipped for a real work package.

Audience: future LLM reviewers updating `LLM/model-routing.md` or broader model-use guidance.

---

```yaml
note_type: field_trial
model_slug: {model-slug}
model_display: {Model Name}
trial_status: testing
date: YYYY-MM-DD
project: project-slug
wp_id: WP-{ID}
handoff_path: LLM/handoffs/WP-{ID}.md
audit_path: LLM/audits/WP-{ID}.md
outcome: completed | stopped | failed | follow_up_required | skipped
skip_reason: null
role_assigned: {trial-role, e.g. conservative_cheap_writer}
competent_default: {default-model-slug}
auditor_model: {auditor-model-slug}
production_risk: low | medium | high
audit_verdict: pass | fail | follow-up-required | not_audited
recommendation: keep_testing | promote_for_role | do_not_use_for_role | insufficient_data
```

## Summary

One or two sentences. What was attempted and the bottom line.

## Task Shape

- **In scope:** files/areas touched
- **Out of scope:** what was avoided
- **Why this model:** quota / human request / role match / fallback

## What Went Well

- Bullet facts only. Tie to handoff acceptance checks where possible.

## What Went Poorly

- Bullet facts only. Missed edge cases, overclaims, weak areas. No speculation.

## Vs Competent Default

- How did this compare to what your default lane for this task shape would normally do?

## Auditor Findings

- Copy or summarize independent audit points relevant to model choice.

## Recommendation For Routing Docs

- **promote_for_role:** suggest adding to default routing for this role with guardrails
- **keep_testing:** need more field evidence
- **do_not_use_for_role:** failed this role shape
- **insufficient_data:** skip or aborted before meaningful signal

## Suggested Routing Change

Optional one-liner for a future reviewer. Leave blank if none.

---

## Skipped Trial Variant

Use when the human chose a competent default instead of the testing lane. **Do not imply the testing model failed.**

```yaml
note_type: skipped_trial
model_slug: {model-slug}
outcome: skipped
skip_reason: quota | rush | human_override | risk_too_high
competent_model_used: {default-model-slug}
wp_id: WP-{ID}
date: YYYY-MM-DD
```

## Summary

Skipped trial. Human used `{competent_model_used}` because `{skip_reason}`. No field evidence for or against `{model_slug}` on this WP.
