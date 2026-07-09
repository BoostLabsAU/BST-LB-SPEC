# Model Routing Guide

How to fill in `LLM/model-routing.md` for your project. Read this once during adoption; the Orchestrator never loads it.

Model names age fast — this guide teaches the method, which doesn't. The worked example at the bottom is a dated snapshot, an illustration rather than a recommendation.

---

## The Big Idea

Most teams have two kinds of model capacity: **abundant lanes** (an editor-included model, a cheap flat-rate bundle, open weights) and **scarce lanes** (premium models with tight quotas or high cost).

The routing that works: **let abundant lanes do the bulk of the building and checking, and spend scarce lanes only on the two things cheap models get wrong** — hard design decisions, and the final OK on changes where a subtle bug is expensive.

This is safe even with mid-tier models because every WP runs under a strict handoff and closes only on independent audit. The workflow supplies the discipline; the model supplies the labor.

## Route by Task Shape, Not Reputation

A model's overall ranking says little about lane fit. Two patterns recur across benchmarks and field use:

- Strong audit/spec-conformance models are often **poor first-pass diagnosticians**. "Why is this broken?" goes to the diagnosis lane first; the premium lane hardens the fix afterwards.
- Strong bulk coders often **shouldn't make architecture calls**. "How should we design this?" goes to the judgment lane even when the implementation later goes to the cheap lane.

So don't ask "which model is best?" — assign models to the lane shapes in `LLM/model-routing.md`: diagnosis, bulk implementation, premium gate, judgment, and (optionally) frontend taste.

## Mapping Your Subscriptions

1. **What do you already pay for?** An IDE-included model is usually your highest-capacity lane — lean on it hardest in its strong stacks.
2. **What's your cheapest competent bulk lane?** Flat-rate or open-weights coders are fine *under mandatory audit*; never as final authority.
3. **What's your strongest model?** That's the premium gate and judgment lane. Protect its quota — no bulk coding, no first-pass log reading.
4. **Which lane diagnoses?** Pick for large-context, multi-file reasoning — and verify it can actually do incident work before defaulting to the premium model out of habit.
5. **Who audits whom?** Fill audit lanes last, honoring the hard rule: a WP's auditor must differ from its coder. If one model fills both lanes, alternate per WP.

Only one or two models? The workflow still works — session separation (fresh Coder, fresh Auditor) matters more than model separation. Mark single-model audits low-assurance and gate high-blast-radius work on the human.

## Trials and Promotion

New model or untrusted cheap lane? Don't rewrite routing on vibes:

1. Add it to `LLM/model-routing.md` marked `Testing`.
2. Route it only WPs matching the trial role at acceptable risk.
3. Keep independent audit on a competent lane — always.
4. Write a field note after each real attempt (`LLM/model-notes/`).
5. Promote to `Default` only on multiple agreeing notes, including at least one medium/high-risk WP for production-sensitive roles.

This builds an evidence trail and makes model churn cheap: a new model enters as a testing lane and earns its slot.

## Worked Example (snapshot: June 2026)

> One real adoption's lane table, kept to illustrate the method. These models were current in mid-2026 and may be superseded by the time you read this — verify before copying anything.

The stack: an editor subscription with an included near-premium coder, a cheap flat-rate bundle covering two models, and one scarce premium subscription.

| Lane | Model | Why |
|---|---|---|
| Diagnosis / root cause | Kimi K2.7 Code | Best available log/incident reasoning; high context; abundant quota. |
| Coder — bulk (Node/PHP) | Kimi K2.7 Code | Abundant; strong implementation under strict handoffs. |
| Coder — editor-included (PHP/WordPress) | Composer 2.5 | Highest usage limits in the stack; strongest PHP lane; not used for diagnosis. |
| Coder — budget fallback | GLM 5.2 | Cheap careful writer for low/medium-risk WPs under mandatory audit. |
| Coder — transactional / concurrency | GPT 5.5 xhigh | The narrow slice where a race-condition bug outcosts the premium rate. |
| Auditor — everyday | Composer 2.5 or GLM 5.2 | Structural/spec review on abundant quota; never audits its own code. |
| Auditor — high-blast-radius gate | GPT 5.5 xhigh | Final OK on money, auth, schema, migrations. |
| Judgment / architecture | Claude Opus | Planning, trade-offs, long-context review, second-opinion audits. |
| Frontend / design taste | Claude Sonnet | Testing lane for CSS/layout; kept out of backend entirely. |

Two details worth copying regardless of models: the premium lanes appear only in the gate/judgment rows, never the bulk rows — and the benchmark leader for audits was deliberately *not* used for diagnosis, because trials showed it failing incident root-cause tasks it was never shaped for.
