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

## Setup for a New Project

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
│   ├── HANDOFF_{FEATURE}.md           ← Coding instructions (The "Skill" prompt)
│   ├── context/                       ← Feature design docs
│   │   └── {feature}.md               ← Requirements, data schemas, command flows
│   ├── completions/                   ← Coding LLM completion reports
│   │   └── {feature}.md               ← Pass/fail, commands run, files explored
│   └── docs/                          ← Living documentation
│       ├── COMMANDS.md                ← All user-facing commands/APIs
│       ├── API_REFERENCE.md           ← Internal function signatures & schemas
│       └── RULES.md                   ← Persistent invariants & bounds
├── ... (your project files)
```

---

## Key Principles

1. **Large, generic global context is often harmful** — The Coding LLM receives a highly-focused procedural handoff (a "Skill"), not a giant repo summary.
2. **Failure Memory, not Architecture Memory** — Global context is restricted to a tiny `RULES.md` file that only tracks persistent failures and non-negotiable conventions.
3. **Controlled Agent Exploration** — The Coding LLM is told exactly where to start reading. If it gets blocked, it is allowed to perform targeted searches, but must stop and ask for clarification if the scope expands too far.
4. **Procedural Guidance Over Declarative Goals** — Handoffs provide step-by-step approaches and exact acceptance checks instead of just saying "build this feature."
5. **Measurable Completion Reports** — Every coding task ends with an evidence-driven report containing pass/fail metrics, exact commands run, extra files explored, and performance tracking if available.
6. **Every handoff is audited** — The Orchestrator verifies against the spec before closing out, measuring task success metrics to prove value.

---

## Changelog

### v2 (2026-02-23)

Design informed by two research papers:
- *Evaluating AGENTS.md* (arXiv:2602.11988) — large generic context files reduce task success and increase cost; minimal human-written context helps.
- *SkillsBench* (arXiv:2602.12670) — curated procedural Skills improve pass rates +16pp; focused 2–3 module Skills outperform comprehensive documentation.

Changes:
- **Deduplicated sources of truth.** `ORCHESTRATOR_PROMPT.md` now defers to `LLM/ORCHESTRATOR_BOOTSTRAP.md` for workflow and `LLM/HANDOFF_TEMPLATE.md` for handoff format. No more duplicated blocks to drift.
- **Added `LLM/docs/RULES.md`** — tiny failure-memory file. Rules added only when the Coding LLM repeats errors; pruned periodically.
- **Improved handoff template.** Replaced rigid "Do NOT read other files" with controlled exploration + required logging. Added "Deviations from Handoff" to completion reports.
- **Emphasized procedural guidance** in handoffs — step-by-step instructions over declarative goals.
- **Cleaned README** — removed promotional framing, kept factual principles.

### v1 (Initial)

- Original two-LLM workflow template with role separation, handoff/completion/audit cycle, and living documentation.
