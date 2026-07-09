# BST-LB-SPEC — Spec-Driven AI Development Workflow

A repo-native, markdown-first workflow for building software with AI agents.

Three execution roles, two optional entrypoints (Deep Planner, and a one-time adoption Scout), six execution stages, one authority chain. Works with Claude Code, Cursor, Codex CLI, Aider, opencode, or any model/harness combination. No build tools, no scripts, no vendor lock-in.

## Start In 30 Seconds

Copy `LLM/` and `ORCHESTRATOR_PROMPT.md` into your project root, then paste one line into a fresh AI session:

- **New project:** `Read LLM/START_ORCHESTRATOR.md and follow it.`
- **Existing project:** `Read LLM/START_SCOUT.md and follow it.` — the Scout maps your repo once, then hands you the Orchestrator kickstart.

Full protocol: [`QUICK_START.md`](QUICK_START.md).

---

## What This Is

```text
requirements → design → task list → handoff → completion → audit
```

Each stage produces a small markdown artifact. Each stage subordinates to the one above it.

Work is delivered in **work packages (WPs)** — units of work sized for one focused coding session. WP IDs are zero-padded, assigned by the Orchestrator in creation order, **globally unique across the whole project** (WP-001, WP-002, …), and never renumbered — so `LLM/handoffs/WP-{ID}.md`, `LLM/completions/WP-{ID}.md`, and `LLM/audits/WP-{ID}.md` never collide, even across multiple task lists.

The flow runs across three execution roles plus two optional entrypoints:

- **Orchestrator (planner / router)** — analyzes, designs, writes handoffs, routes audits, and maintains documentation. **Does not write production code.**
- **Coder (executor)** — reads one handoff, implements exactly that scope, writes a completion report and return prompt. **Does not redefine requirements or design.**
- **Auditor (reviewer)** — reviews an audit-ready completion in a separate session and writes the audit verdict. **Does not implement the handoff.**
- **Deep Planner (optional)** — decomposes large or ambiguous work before the Orchestrator issues handoffs.
- **Scout (optional, one-time)** — explores an existing repo at adoption time, records authority docs and verification commands, and hands you a ready Orchestrator kickstart.

For serious work, run Orchestrator/Coder/Auditor as separate sessions. The high-trust default is a separate Auditor session. One session role-playing every role is low-assurance.

---

## Role Start Files

The drop-in `LLM/` folder includes five role-specific starts:

| Role | Start file | Use when |
|---|---|---|
| Orchestrator | `LLM/START_ORCHESTRATOR.md` | planning, writing handoffs, receiving completions, routing audits, updating live state |
| Coder | `LLM/START_CODER.md` | implementing exactly one active handoff |
| Auditor | `LLM/START_AUDITOR.md` | reviewing an audit-ready completion |
| Deep planner | `LLM/START_DEEP_PLANNER.md` | decomposing large/ambiguous work before handoff |
| Scout (one-time) | `LLM/START_SCOUT.md` | adopting BST-LB-SPEC on an existing repo you haven't mapped yet |

`LLM/START_HERE.md` is only a router to these files.

---

## The Authority Chain

When two layers conflict, the higher layer wins.

| # | Layer | Lives in | Owner |
|---|---|---|---|
| 1 | Requirements | `LLM/requirements/` or external authority docs | Orchestrator + human |
| 2 | Design | `LLM/design/` or external authority docs | Orchestrator + human |
| 3 | Tasks | `LLM/tasks/` | Orchestrator |
| 4 | Handoffs | `LLM/handoffs/` | Orchestrator |
| 5 | Completions | `LLM/completions/` | Coder |
| 6 | Audits | `LLM/audits/` | Auditor or Orchestrator acting as Auditor |
| 7 | Rules | `LLM/rules.md` | Orchestrator |

Rules:

- `rules.md` cannot override requirements or design.
- Handoffs cannot override tasks.
- Completions cannot redefine scope.
- Audits can fail work, but cannot silently change requirements.
- **Audit failure blocks merge/close.**

---

## State Files: `CURRENT.md` vs `SPEC.md`

BST-LB-SPEC intentionally separates live volatile state from slow authority/index state.

- `LLM/CURRENT.md` is the live active-work pointer. The Orchestrator updates it after every workflow transition.
- `LLM/SPEC.md` is a slow-moving authority/index file. Do not use it as a per-WP active tracker.

Allowed `CURRENT.md` statuses:

- `idle; awaiting orchestrator selection`
- `awaiting-coder`
- `awaiting-completion`
- `awaiting-audit`
- `blocked; awaiting orchestrator decision`
- `follow-up awaiting coder`
- `complete`

State ownership:

- Orchestrator owns `CURRENT.md` transitions.
- Coder writes completion evidence and the Orchestrator Return Prompt.
- Auditor writes audit verdicts.
- Per-WP closure truth lives in `LLM/audits/WP-{ID}.md`.
- After Pass audits, the Orchestrator moves the WP to `CURRENT.md#Recently Completed`, populates `CURRENT.md#Next`, and immediately issues the next handoff/Coder prompt unless the human pauses or no work remains.
- Missing/corrupt completions reset to coder handoff; blocked/partial/stopped completions go to Orchestrator blocker handling, not audit.

---

## The Coder Return Contract

Every coding session ends the same way, no matter how it went: the Coder writes a completion report to `LLM/completions/`, and both the report and its final chat message end with a short paste-ready block — the **Orchestrator Return Prompt**. You copy that block into the Orchestrator window, and the loop continues.

This is what keeps the workflow closed. Whether the Coder finished cleanly, failed verification, hit a stop condition, or got blocked halfway, control always returns to the Orchestrator with a structured summary: what happened, what was verified, whether the work is ready for audit, and what decision is needed next. A session that ends without the return prompt is treated as incomplete — the Orchestrator requests a corrected report instead of guessing what happened.

---

## Two Modes

Pick one in `LLM/SPEC.md` when you adopt. If you're not sure which fits, the Scout session picks for you.

### New Project Mode

You have little or no existing planning, so BST-LB-SPEC owns the full chain:

```text
requirements → design → tasks → handoffs → completions → audits
```

Start at the top with `LLM/REQUIREMENTS_TEMPLATE.md`; each layer flows from the one above it.

### Existing Project Mode

Your repo already has docs that *function as* requirements and design — architecture docs, ADRs, an implementation plan, even a thorough README. **Do not duplicate them.** They stay where they are and remain the authority; handoffs cite them directly.

BST-LB-SPEC adds only the execution layers your project is missing. Most often that is:

```text
tasks → handoffs → completions → audits
```

List your authority docs in `LLM/SPEC.md` — or run the one-time Scout session (`LLM/START_SCOUT.md`), which explores the repo, fills `SPEC.md` for you, writes `LLM/design/baseline.md` if you have no usable architecture docs, and hands you the Orchestrator kickstart.

### Hybrid Mode

A mix: some layers come from external docs, others are authored under `LLM/` — e.g. design lives in your `docs/` folder, but a new feature gets a scoped requirements bridge under `LLM/requirements/`. Mark which is which in `LLM/SPEC.md` so handoffs cite the right source.

---

## Create The Next Missing Layer Only

The six stages form a ladder: requirements → design → tasks → handoffs → completions → audits. Most projects already have some of the upper rungs in some form — a README that acts as requirements, architecture docs that act as design, maybe a task tracker.

The rule: **find the highest rung your project is missing, and create only that one.** Never recreate a layer that already exists somewhere else — cite it instead.

- Project has nothing? Start at the top: author requirements.
- Has requirements and design (in any form)? Create the task list.
- Has a task list too? Go straight to handoffs, completions, and audits — this is where most existing projects land.
- One feature too vague to hand off safely? Write a small, feature-scoped requirements or design "bridge" for just that feature, then task it. Bridges stay subordinate to your real docs.

This one rule is what keeps BST-LB-SPEC a thin execution layer instead of a second planning system competing with your actual documentation.

---

## Folder Structure

```text
this-template/                          # copy LLM/ and ORCHESTRATOR_PROMPT.md into your project root
├── QUICK_START.md                      # quick start and copy-paste protocol (read from the template)
├── MODEL_ROUTING_GUIDE.md              # adoption-time guide for filling LLM/model-routing.md
├── ORCHESTRATOR_PROMPT.md              # kickstart prompt for Window 1 (copy into your project)
├── examples/                           # one complete WP lifecycle: task list → handoff → completion → audit
├── LLM/
│   ├── START_HERE.md                   # router to role start prompts
│   ├── START_ORCHESTRATOR.md           # orchestrator entrypoint
│   ├── START_CODER.md                  # coder entrypoint
│   ├── START_AUDITOR.md                # auditor entrypoint
│   ├── START_DEEP_PLANNER.md           # deep-planning/decomposition entrypoint
│   ├── START_SCOUT.md                  # one-time adoption/intake entrypoint
│   ├── RESUME_PROMPT.md                # context-reset / resume protocol
│   ├── SPEC.md                         # spec index + authority order + current mode
│   ├── CURRENT.md                      # tiny active-work pointer
│   ├── ORCHESTRATOR_BOOTSTRAP.md       # authoritative orchestrator behavior
│   ├── context-loading.md              # role-based context loading policy
│   ├── model-routing.md                # optional model/lane routing guidance
│   ├── REQUIREMENTS_TEMPLATE.md
│   ├── DESIGN_TEMPLATE.md
│   ├── TASKLIST_TEMPLATE.md
│   ├── HANDOFF_TEMPLATE.md
│   ├── COMPLETION_TEMPLATE.md
│   ├── AUDIT_TEMPLATE.md
│   ├── rules.md                        # failure memory only
│   ├── requirements/                   # if BST-LB-SPEC owns requirements or scoped bridges
│   ├── design/                         # if BST-LB-SPEC owns design or scoped bridges
│   ├── tasks/
│   ├── handoffs/
│   ├── completions/
│   ├── audits/
│   ├── model-notes/                    # field-trial notes for testing lanes
│   └── skills/                         # reusable patterns; created at 3+ repeats
└── ... project files
```

You do not need every folder immediately. Create artifacts as you reach the layer that needs them.

---

## How An Audit Works

The audit is a separate artifact in `LLM/audits/`, not a step buried inside the orchestrator's response.

1. Coder finishes/stops a handoff and writes the completion report.
2. Completion includes `## Orchestrator Return Prompt`.
3. Orchestrator reads the completion.
4. If `Status: Pass` and `Ready For Audit? Yes`, Orchestrator sets `CURRENT.md` to `awaiting-audit` and routes to a fresh Auditor by default.
5. Auditor reads the diff, completion, handoff, task entry, and cited authority docs.
6. Auditor runs verification commands when possible.
7. Auditor writes verdict — Pass / Fail / Follow-up required — to `LLM/audits/WP-{ID}.md`.
8. **Pass:** branch can merge/close; the Orchestrator moves the WP to `CURRENT.md#Recently Completed`, populates `CURRENT.md#Next`, and immediately issues the next handoff/Coder prompt unless the human pauses or no work remains.
9. **Fail / Follow-up:** branch stays open; Orchestrator writes a follow-up handoff and updates `CURRENT.md` to follow-up/blocker state. No merge/close.

If the completion is not audit-ready, do not audit it as complete. Resolve the blocker or write a follow-up handoff.

---

## Why `rules.md` Is Failure Memory Only

Architecture, conventions, file locations, and project facts belong in `design/` or your existing authority docs. They do not belong in `rules.md`.

`rules.md` exists for one reason: when an AI agent makes the same mistake twice, write a rule to prevent the third time. Keep it tiny. Prune stale rules.

The template ships it nearly empty: three workflow invariants (marked exempt from pruning) and nothing else. Your project's failure rules accumulate below them.

---

## Quick Start

See `QUICK_START.md` for new-project and existing-project quick starts.

---

## Three Roles, Three Sessions

For high-trust work, run Orchestrator, Coder, and Auditor as **separate sessions**:

| Window | Role | Session policy |
|---|---|---|
| 1 | Orchestrator | Persistent. Keep open across WPs. |
| 2 | Coder | Fresh session every handoff. Close after return prompt. |
| 3 | Auditor | Fresh session every audit. Close after verdict. |

The human copy-pastes prompts between windows:

1. Orchestrator writes handoff → gives Coder prompt → human pastes into fresh Coder window
2. Coder implements → returns `## Orchestrator Return Prompt` → human pastes into Orchestrator window
3. Orchestrator processes completion → gives Auditor prompt → human pastes into fresh Auditor window
4. Auditor writes verdict → human pastes verdict summary into Orchestrator window
5. Orchestrator updates state → writes next handoff → repeat

The Coder never audits its own work. The Auditor never shares context with the Coder. The Orchestrator never reads the Coder's chat transcript — only the completion report on disk and the structured return prompt the human pastes in.

See `QUICK_START.md` for the full protocol diagram.

---

## Which LLMs

Any. The workflow is model-agnostic: the handoff/audit discipline is what carries quality, not a specific model. Pick an Orchestrator, a Coder, and an Auditor from whatever your subscriptions include.

`LLM/model-routing.md` is the optional cost/risk routing layer. It ships model-agnostic: it maps whatever models you have onto lane shapes (diagnosis, bulk implementation, premium gate, judgment) and defines how to trial a new model before trusting it with default routing. Fill in its lane table during adoption — `MODEL_ROUTING_GUIDE.md` explains the method and includes a dated worked example.

Two rules survive any model lineup:

1. The Coder and Auditor of the same WP must be different models — at minimum different sessions.
2. Scarce premium capacity goes to design decisions and final gates on high-blast-radius changes, not bulk implementation.

---

## Skills (Reusable Patterns)

`LLM/skills/` exists for patterns that repeat across handoffs. **Do not pre-create skills.** The Orchestrator extracts a skill the third time it writes the same procedural steps. Two occurrences is a coincidence.

A handoff references at most 0–3 skills. If you'd reference 4+, consolidate or inline only what's needed.

---

## What This Is Not

- Not OpenSpec, Spec Kit, GSD, BMAD, Notion, Linear, or a process-heavy PM framework.
- Not a CLI, package, or build tool. Markdown only.
- Not a replacement for your existing architecture docs. Use them — point handoffs at them.
- Not opinionated about which AI harness you use. Pick whichever respects file scope and verification.

---

## Changelog

- **v2.0 (current)** — Renamed to **BST-LB-SPEC**. Spec-driven rework: separated Orchestrator / Coder / Auditor roles plus Deep Planner and one-time Scout entrypoints; the authority chain (requirements → design → tasks → handoffs → completions → audits); audits as separate artifacts with fail = no merge; `CURRENT.md`/`SPEC.md` state split; mandatory Orchestrator Return Prompt on every Coder outcome; model-agnostic routing layer with field-trial notes; globally unique WP IDs; worked example in `examples/`.
- **v1** — the original two-LLM (Orchestrator + Coder) template, published as `LLM-Orchestrator-coder-setup`.

Upgrading from v1: the old `LLM/context/`, `LLM/CURRENT_TASKS.md`, `LLM/orchestrator_notes.md`, `LLM/docs/`, and `LLM/scout/` layers are deprecated — do not recreate them. Their jobs moved into `LLM/SPEC.md`, `LLM/CURRENT.md`, `LLM/rules.md`, and the one-time Scout session.
