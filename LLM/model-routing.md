# Model Routing (Optional)

Status: optional cost/risk routing layer. Not product authority — requirements, design, task lists, and handoffs still define the work; audit verdicts still decide closure.

Last updated: fill in during adoption

Only the Orchestrator loads this file, and only when a handoff, audit prompt, or deep-planning prompt needs a model/lane recommendation. Fill the lane table during adoption with the models your subscriptions actually include — `MODEL_ROUTING_GUIDE.md` (in the BST-LB-SPEC template repo) explains how and includes a dated worked example.

## Two Rules That Always Apply

1. **Coder ≠ Auditor on the same work.** No model grades its own homework.
2. **Spend scarce premium capacity only where cheap lanes are weak:** hard design decisions and the final check on high-blast-radius changes. Abundant lanes carry the implementation volume.

## Lane Shapes

Map your models onto these shapes. One model may fill several lanes; rule 1 still binds.

| Lane | What it does | Route here when |
|---|---|---|
| Diagnosis | Root-cause from logs and incidents | The cause is unknown — diagnose first, then hand the known cause to a coder lane. |
| Bulk implementation | High-volume coding under a strict handoff | Scope and cause are known; risk is low/medium; audit is guaranteed. |
| Premium gate | Final audit and high-risk implementation | Money, auth, schema, concurrency — anywhere a subtle bug is expensive. |
| Judgment / architecture | Planning, design trade-offs, second opinions | The question is "how should we build this?", not "build this." |
| Frontend / design taste (optional) | CSS, layout, visual polish | The work is visual and your bulk lane's taste is weak. |

Route by shape, not overall reputation — strong audit models are often weak first-pass diagnosticians, and strong bulk coders shouldn't make architecture calls.

## Your Lane Table

Fill in during adoption. Mark each lane `Default` or `Testing`.

| Role / Need | Model | Status | Guardrails |
|---|---|---|---|
| Orchestrator / planning | | | |
| Coder — bulk implementation | | | |
| Coder — high-risk / transactional | | | |
| Auditor — everyday review | | | |
| Auditor — high-blast-radius gate | | | |
| Diagnosis / root cause | | | |

## High-Blast-Radius Work

Whatever the lane table says, require your strongest audit lane as the final gate for: payments and money paths, auth and sessions, user data, schema changes and migrations, redirects/cache/routing behavior, and release or deploy configuration. Do not close these WPs on a budget-lane audit.

## Testing Lanes

A **testing lane** is a model you are trialing before trusting it with default routing.

1. **Default first** — write the handoff for a competent default lane.
2. **Testing assignment only when** the human asks for a trial, quota forces an acceptable-risk fallback, or the task shape matches the trial role.
3. **The handoff must say so** — `## Model / Lane` names the model, marks it `Testing`, states why, and names the competent fallback.
4. **Audit unchanged** — testing-lane work still gets an independent audit on a competent lane.
5. **Field note required** — after a testing lane completes or materially attempts a real WP, the Orchestrator writes a note (below). Tell the human in chat when a result is strong or surprising.
6. **Promotion threshold** — promote only when multiple field notes agree on the same `role_assigned` and `recommendation: promote_for_role`, including at least one medium/high-risk note unless the role is inherently low-risk.

## Field Trial Notes

- **Directory:** `LLM/model-notes/` · **Template:** `LLM/model-notes/NOTE-TEMPLATE.md`
- **Filename:** `{model-slug}-{YYYY-MM-DD}-{wp-id-or-short-topic}.md`

If the human skips a planned trial (rush, quota, risk), never write a note implying the model failed — optionally record a `skipped_trial` note. A skip is not evidence against the model.
