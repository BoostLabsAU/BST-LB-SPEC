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
6. **Orchestrator → You:** Performs code review. If issues exist, gives follow-up prompt. If passed, updates docs and asks "What next?"

---

## Folder Structure

```
your-project/
├── LLM/                              ← AI workflow folder
│   ├── ORCHESTRATOR_BOOTSTRAP.md      ← Project overview for the Orchestrator
│   ├── orchestrator_notes.md          ← Running log of all changes (auto-maintained)
│   ├── CURRENT_TASKS.md               ← Active/completed task tracker
│   ├── HANDOFF_{FEATURE}.md           ← Generated per feature (coding instructions)
│   ├── context/                       ← Feature design docs
│   │   └── {feature}.md               ← Requirements, data schemas, command flows
│   ├── completions/                   ← Coding LLM completion reports
│   │   └── {feature}.md               ← What was changed, verification results
│   └── docs/                          ← Living documentation
│       ├── COMMANDS.md                ← All user-facing commands/APIs
│       └── API_REFERENCE.md           ← Internal function signatures & schemas
├── ... (your project files)
```

---

## Key Principles

1. **Orchestrator never writes production code** — it plans, designs, audits, and documents
2. **Coding LLM never reads unnecessary files** — handoffs list exactly which files to read
3. **Every handoff is self-contained** — the coding LLM should be able to implement without asking questions
4. **Every implementation is audited** — the orchestrator verifies against the spec before closing out
5. **Documentation stays current** — updated after every audit, not as an afterthought
6. **The handoff includes verification steps** — so the coding LLM proves its work
7. **The handoff includes completion report format** — standardized output for audit
