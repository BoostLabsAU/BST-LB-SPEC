# BST-STRUCTURE — Two-LLM Development Workflow

## What This Is
A portable project structure for building software using two AI agents:
1. **Orchestrator LLM** (Planner) — analyzes the project, designs features, writes handoff prompts, audits implementations, maintains documentation
2. **Coding LLM** (Executor) — reads handoff prompts, implements the code, writes completion reports

This separation prevents scope creep, keeps implementations focused, and creates an audit trail.

---

## How It Works

```
┌─────────────────────────────────────────────────────┐
│                    YOU (Human)                       │
│  "I want to add user authentication"                │
└─────────────────┬───────────────────────────────────┘
                  │
                  ▼
┌─────────────────────────────────────────────────────┐
│              ORCHESTRATOR LLM (Planner)              │
│                                                      │
│  1. Asks clarifying questions                        │
│  2. Creates LLM/context/{feature}.md                 │
│  3. Creates LLM/HANDOFF_{FEATURE}.md                 │
│  4. Gives you the prompt to paste into Coding LLM    │
└─────────────────┬───────────────────────────────────┘
                  │ You paste the prompt
                  ▼
┌─────────────────────────────────────────────────────┐
│               CODING LLM (Executor)                  │
│                                                      │
│  1. Reads the handoff + reference files              │
│  2. Implements the code                              │
│  3. Runs syntax/verification checks                  │
│  4. Writes LLM/completions/{feature}.md              │
│  5. Gives you the audit prompt to paste back         │
└─────────────────┬───────────────────────────────────┘
                  │ You paste the audit prompt
                  ▼
┌─────────────────────────────────────────────────────┐
│              ORCHESTRATOR LLM (Auditor)              │
│                                                      │
│  1. Reads completion report                          │
│  2. Performs Code Review against handoff spec        │
│  3. Runs syntax checks                              │
│  4. Writes follow-up prompt to fix issues (if needed)│
│  5. Updates documentation (if passed)                │
│  6. Asks "What next?"                                │
└─────────────────────────────────────────────────────┘
```

---

## Which LLMs Should I Use?

We tested 20+ models across 3 waves of cross-audit benchmarks (~16,000 data points). Recommended pairs:

| Tier | Orchestrator | Coder |
|-------------|-------------|-------|
| Best overall | GPT-5.2 | Codex 5.3 |
| Best value | Sonnet 4.6 | Codex 5.3 |
| Cheapest (self-host / open weights) | DeepSeek V3.2 | Kimi 2.5 |

**Main recommendation: GPT-5.2 (Orchestrator) + Codex 5.3 (Coder).**

*Note: “Cheapest (self-host / open weights)” reflects our benchmark runs; pricing changes over time and varies by provider.*

Full rationale and methodology: [LLM Recommendations](evals/LLM_RECOMMENDATIONS.md) *(detailed internal analysis is archived in `testing-internal/`)*.

---

## Setup for a New Project

### 0. Pick your Orchestrator and Coder LLMs
See the table above or read [evals/LLM_RECOMMENDATIONS.md](evals/LLM_RECOMMENDATIONS.md).

### 1. Copy the `LLM/` folder template into your project root
### 2. Bootstrap the Orchestrator

Paste this into your Orchestrator LLM:

> You are the **Orchestrator LLM** for this project. Read the file `LLM/ORCHESTRATOR_BOOTSTRAP.md` to understand the project structure, existing features, and your workflow. Then ask me what I'd like to work on.

### 3. For each feature, the cycle is:

1. **You → Orchestrator:** Describe the feature
2. **Orchestrator → You:** Asks questions, then writes handoff
3. **You → Coding LLM:** Paste the handoff prompt
4. **Coding LLM → You:** Implements + gives audit prompt
5. **You → Orchestrator:** Paste the audit prompt
6. **Orchestrator -> You:** Performs code review. If issues exist, gives follow-up prompt. If repeated issues emerge, updates `LLM/docs/RULES.md`. If passed, updates docs and asks "What next?"

---

## Folder Structure

```
your-project/
├── LLM/                              ← AI workflow folder
│   ├── ORCHESTRATOR_BOOTSTRAP.md      ← Project overview for the Orchestrator
│   ├── orchestrator_notes.md          ← Running log of all changes
│   ├── CURRENT_TASKS.md               ← Active/completed task tracker
│   ├── HANDOFF_{FEATURE}.md           ← Coding instructions (handoff prompt)
│   ├── context/                       ← Feature design docs
│   │   └── {feature}.md               ← Requirements, data schemas, command flows
│   ├── completions/                   ← Coding LLM completion reports
│   │   └── {feature}.md               ← Pass/fail, commands run, files explored
│   ├── docs/                          ← Living documentation
│   │   ├── COMMANDS.md                ← All user-facing commands/APIs
│   │   ├── API_REFERENCE.md           ← Internal function signatures & schemas
│   │   └── RULES.md                   ← Persistent invariants & bounds
│   └── skills/                         ← Reusable procedural patterns (created over time)
│       └── {pattern}.md                ← e.g., add-api-endpoint.md, write-unit-tests.md
├── ... (your project files)
```

---

## Key Principles

1. **Large, generic global context is often harmful** — The Coding LLM receives a highly-focused procedural handoff, not a giant repo summary.
2. **Failure Memory, not Architecture Memory** — Global context is restricted to a tiny `RULES.md` file that only tracks persistent failures and non-negotiable conventions.
3. **Controlled Agent Exploration** — The Coding LLM is told exactly where to start reading. If it gets blocked, it is allowed to perform targeted searches, but must stop and ask for clarification if the scope expands too far.
4. **Procedural Guidance Over Declarative Goals** — Handoffs provide step-by-step approaches and exact acceptance checks instead of just saying "build this feature."
5. **Measurable Completion Reports** — Every coding task ends with an evidence-driven report containing pass/fail metrics, exact commands run, extra files explored, and performance tracking if available.
6. **Every handoff is audited** — The Orchestrator verifies against the spec before closing out, measuring task success metrics to prove value.
7. **Reusable Skills over repeated instructions** — When the Orchestrator writes the same procedural steps three times, it extracts them into a `LLM/skills/` file. Handoffs reference a small number of relevant skills (0–3) instead of repeating boilerplate.

---

## Optional: Skill Feedback CSV

Skills are created/updated by the Orchestrator, but it’s still useful to periodically **prune/repair** skills based on real usage feedback (since Skills can sometimes hurt).

The lightweight option (no scripts):

```powershell
rg -n "^- \\*\\*Skills Used" LLM/completions
```

If you want a spreadsheet-friendly CSV, use an external helper script (kept as a Gist so this repo stays “drop-in” and tool-agnostic):

```powershell
$raw = "https://gist.githubusercontent.com/BoostLabsAU/813cec00dbc21da9e61ec44adb02f0ae/raw/0bd5cb6c786316db2e0aa0a8cbeea0f30953d3be/collect_skill_feedback.py"
Invoke-WebRequest -Uri $raw -OutFile ".\\collect_skill_feedback.py"
python .\\collect_skill_feedback.py --completions-dir LLM\\completions --out evals\\skills_log.csv
```

It writes `evals/skills_log.csv` (gitignored). Open it in Sheets/Excel and filter by skill to find “confusing” / “harmful” patterns, then update or prune `LLM/skills/*.md`.

---

## See Also

- **[DevChain](https://github.com/twitech-lab/devchain)** — A more advanced multi-agent development framework with structured task pipelines. If you need something beyond a lightweight template, worth checking out.

---

## Changelog

### v3.1 (2026-03-02)

- **Skills are selective (0–3 per handoff).** Avoid 4+ skill sprawl; never ask the Coding LLM to author skills mid-task.
- **Completion reports capture skill impact.** Added “Skills Used (if any)” with “helpful/confusing” feedback.
- **Verification defaults to focused.** Prefer 1–3 commands unless the handoff requires broader testing.
- **Optional skill-feedback CSV workflow.** `evals/skills_log.csv` can be generated locally (gitignored) using a Gist-based helper script, or by grepping completion reports.

### v3 (2026-03-02)

- **Added LLM pair recommendations.** Ran a 3-wave cross-audit benchmark (Node.js, Laravel, Rails) with 20+ models. Summary published in `evals/LLM_RECOMMENDATIONS.md` (detailed internal analysis archived in `testing-internal/`).
- **Recommended default pair:** Sonnet 4.6 (Orchestrator) + Codex 5.3 (Coder).
- **Added "Which LLMs Should I Use?" section to README** with a quick-reference table.
- **Cleaned project structure** — internal testing data moved to `testing-internal/` and excluded from git.

### v2.1 (2026-02-23)

Design informed by:
- *Harness Engineering: Leveraging Codex in an Agent-First World* (OpenAI, Feb 2026) — repository-embedded skills, depth-first decomposition, focused sub-documents over monolithic instruction files.

Changes:
- **Added `LLM/skills/` directory** — reusable procedural instruction fragments. Created by the Orchestrator when handoff patterns repeat 3+ times. Keeps handoffs DRY without bloating global context.
- **Updated Orchestrator workflow** — new step to reference/create skills during handoff writing.
- **Updated Handoff template** — skills can be listed under "Read These Files First" when applicable.
- **Updated Orchestrator Prompt & Quick Start** — added skills to documentation table and pro tips.
- **Fixed terminology** — removed ambiguous "Skill prompt" phrasing that conflicted with the new skills concept.
- **Adopted minor versioning** — additive changes use v2.x; major redesigns use v3.

### v2 (2026-02-23)

Design informed by two research papers:
- *Evaluating AGENTS.md* (arXiv:2602.11988) — large generic context files reduce task success and increase cost; minimal human-written context helps.
- *SkillsBench* (arXiv:2602.12670) — curated procedural Skills improve pass rates (avg +16.2pp across domains; +4.5pp on SWE; 16/84 tasks show negative deltas). Focused 2–3 Skills outperform 4+ Skills and “comprehensive” documentation.

Changes:
- **Deduplicated sources of truth.** `ORCHESTRATOR_PROMPT.md` now defers to `LLM/ORCHESTRATOR_BOOTSTRAP.md` for workflow and `LLM/HANDOFF_TEMPLATE.md` for handoff format. No more duplicated blocks to drift.
- **Added `LLM/docs/RULES.md`** — tiny failure-memory file. Rules added only when the Coding LLM repeats errors; pruned periodically.
- **Improved handoff template.** Replaced rigid "Do NOT read other files" with controlled exploration + required logging. Added "Deviations from Handoff" to completion reports.
- **Emphasized procedural guidance** in handoffs — step-by-step instructions over declarative goals.
- **Cleaned README** — removed promotional framing, kept factual principles.

### v1 (Initial)

- Original two-LLM workflow template with role separation, handoff/completion/audit cycle, and living documentation.
