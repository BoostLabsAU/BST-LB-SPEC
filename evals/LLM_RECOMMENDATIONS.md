# LLM Recommendations — BST-STRUCTURE

**Date:** 2026-03-02  
**Scope:** Waves 1-3 (~16,000+ cross-audit data points, 20+ models)

| Tier | Orchestrator | Coder | Why This Pair |
|-------------|--------------|-------|---------------|
| Best overall | GPT-5.2 | Codex 5.3 | Strongest judge + strongest implementer: GPT-5.2 is S-tier auditor (bias **-0.86**, spread **31**); Codex 5.3 is a top-3 coder (**36.50/40**) and S-tier reviewer (bias **-1.50**, spread **33**). |
| Best value | Sonnet 4.6 | Codex 5.3 | Best value: Sonnet stays calibrated across waves (bias **0.0** in Wave 1+2; **-2.86** in Wave 3); Codex 5.3 ships consistent code (**36.50/40**) under guardrails. |
| Cheapest (self-host / open weights) | DeepSeek V3.2 | Kimi 2.5 | Lowest-cost pair we tested that still produced usable guardrails: DeepSeek is one of the only honest budget reviewers (bias **+0.91** in Wave 3); Kimi is the best cheap builder when guided (**38/40** in Wave 1+2). |

**Main recommendation:** GPT-5.2 (Orchestrator) + Codex 5.3 (Coder).

*Note: “Cheapest (self-host / open weights)” reflects our benchmark runs; pricing changes over time and varies by provider.*

**Laravel-heavy alternative:** Sonnet 4.6 + Opus 4.6 (Opus is the top Laravel specialist: **39.20/40** Laravel avg; **36.68/40** overall in Wave 3).

## GPT-5.2 vs Opus 4.6 (Interchangeable? Mostly, with tradeoffs)

For quality-critical work, **GPT-5.2 and Opus 4.6 are broadly interchangeable** as the Orchestrator in BST-STRUCTURE: both can plan, write guardrails, and catch subtle issues. The best choice depends on what you optimize for.

**GPT-5.2 strengths**
- Consistently excellent **auditor calibration** (good discrimination, low inflation) and strong across stacks.
- Very strong at **tight spec compliance** and “judge” behavior (calling out missing verification, unclear acceptance criteria).

**GPT-5.2 weaknesses**
- Can be more “procedure heavy” unless you keep handoffs short; be strict about 0–3 skills and focused verification.

**Opus 4.6 strengths**
- Excellent **bug-finding / review quality** in practice, and particularly strong on **Laravel-heavy** work.
- Often produces very actionable audits (concrete fixes, good prioritization).

**Opus 4.6 weaknesses**
- Like any strong model, can still over-elaborate; keep the Orchestrator constrained to the handoff template and don’t let it bloat global docs.

**As a Coder (implementer)**
- **Opus 4.6** is a strong coder and a good choice when you want the implementer to follow nuanced procedural guidance closely (especially in Laravel-heavy repos).
- **GPT-5.2** can also code at a high level, but in this workflow it’s usually higher leverage as the judge/Orchestrator while a specialist coder (e.g., Codex 5.3) does execution.

Practical rule of thumb:
- Default to **GPT-5.2** as Orchestrator for general use.
- Use **Opus 4.6** as Orchestrator when the codebase is Laravel-heavy or you want maximum “audit sharpness” on tricky bugs.

## Models to Avoid

- Gemini 2.5 Pro (coder): **12.05/40** in Wave 3; failed Rails submission.
- Gemini 3 Flash (orchestrator/auditor): F-tier in Wave 3 (12/13 at **40/40**; bias **+11.64**).
- Grok-4 (orchestrator/auditor): invalid auditor (scored only **2/13** models).
- GLM-5 (orchestrator/auditor): bias **+9.18** and score compression.
- Haiku 4.5: bias **+11.6** self-inflation.
- MiniMax-M2.5 / Codestral: perfect-score inflation => no review signal.

## How We Tested (overview)

We ran a 3-wave cross-audit benchmark (Waves 1-3). A "wave" is just an evaluation round we ran at a different time with a fresh task set (same methodology, different tasks/stacks), so results are less likely to be overfit to one narrow suite.

In each wave, 20+ models:
- implemented the same set of scoped coding tasks (across Node.js, Laravel, and Rails over the full set of waves), and
- audited peers' submissions using the same rubric.

We ranked coders by peer consensus scores (self-scores excluded) and ranked orchestrators by audit discrimination (spread), weak-target detection, and self-bias (inflation). "Wave 3" numbers are the most recent round; "Wave 1+2" are earlier rounds pooled together.
