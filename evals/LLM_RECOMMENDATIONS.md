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

## Models to Avoid

- Gemini 2.5 Pro (coder): **12.05/40** in Wave 3; failed Rails submission.
- Gemini 3 Flash (orchestrator/auditor): F-tier in Wave 3 (12/13 at **40/40**; bias **+11.64**).
- Grok-4 (orchestrator/auditor): invalid auditor (scored only **2/13** models).
- GLM-5 (orchestrator/auditor): bias **+9.18** and score compression.
- Haiku 4.5: bias **+11.6** self-inflation.
- MiniMax-M2.5 / Codestral: perfect-score inflation => no review signal.

## How We Tested (1 line)

Cross-audit benchmark where every model coded and audited peers; the detailed analysis artifacts are archived under `testing-internal/`.
