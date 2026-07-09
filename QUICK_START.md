# Quick Start

## TL;DR

Copy `LLM/` and `ORCHESTRATOR_PROMPT.md` into your project root, then paste one line into a fresh AI session:

- **New project:** `Read LLM/START_ORCHESTRATOR.md and follow it.`
- **Existing project:** `Read LLM/START_SCOUT.md and follow it.` — the Scout maps your repo once, then hands you the Orchestrator kickstart.

The rest of this file is the full protocol for both paths.

---

## Step 0 — Pick your LLMs

You need at least an Orchestrator and a Coder; a separate Auditor is the high-trust default. Any models work — the handoff/audit discipline carries the quality, not a specific model.

If you run several models and want cost/risk routing between them, fill in the lane table in `LLM/model-routing.md` during adoption. `MODEL_ROUTING_GUIDE.md` explains the method and includes a dated worked example.

For high-trust work, run Orchestrator, Coder, and Auditor as separate sessions. One session role-playing all roles is low-assurance.

---

## Session Protocol — Three Standing Windows

Day-to-day work runs in three separate LLM sessions (windows/tabs/conversations); the Deep Planner and Scout are occasional extras:

| Window | Role | Session policy | When to start |
|---|---|---|---|
| 1 | Orchestrator | **Persistent.** Keep open across WPs. Resume with `LLM/RESUME_PROMPT.md` if context gets long. | Start once. Keep open. |
| 2 | Coder | **Fresh session every handoff.** Close after writing the completion report and return prompt. | Start a new session each time the Orchestrator gives you a Coder prompt. |
| 3 | Auditor | **Fresh session every audit.** Close after writing the verdict. | Start a new session each time the Orchestrator gives you an Auditor prompt. |
| as needed | Deep Planner | **Fresh session per planning engagement.** Close after the task list or bridge is written. | When the Orchestrator says the next work is too large or ambiguous to hand off safely. |
| once, at adoption | Scout | **One-time fresh session.** Close after `SPEC.md` and the kickstart are written. | When adopting BST-LB-SPEC on an existing repo you haven't mapped yet. |

Why separate sessions:

- **Coder and Auditor must be different sessions.** The Auditor judges the Coder's work. If they share context, the Auditor has already seen the implementation thinking and cannot give an independent review. Mark the audit low-assurance if one session role-plays both.
- **Coder sessions must be fresh.** A Coder session carries context from that one handoff only. Starting fresh prevents scope creep from prior WPs and keeps the `Read First` discipline tight.
- **Orchestrator sessions are persistent.** The Orchestrator writes handoffs, processes completions, and routes audits. It needs continuity across WPs. If context gets long, resume in a fresh window with `LLM/RESUME_PROMPT.md` — the Orchestrator rehydrates from `LLM/CURRENT.md` and `LLM/ORCHESTRATOR_BOOTSTRAP.md`.

The human copy-pastes prompts between windows:

```text
┌──────────────┐     Coder prompt      ┌──────────────┐
│              │ ─────────────────────►│              │
│  Window 1    │                       │  Window 2    │
│  Orchestrator│◄─────────────────────│  Coder       │
│              │   Return prompt       │  (fresh each │
│              │                       │   handoff)   │
│              │                       └──────────────┘
│              │
│              │   Auditor prompt      ┌──────────────┐
│              │ ─────────────────────►│              │
│              │                       │  Window 3    │
│              │◄─────────────────────│  Auditor      │
│              │   Audit verdict       │  (fresh each │
│              │                       │   audit)     │
└──────────────┘                       └──────────────┘
```

The Orchestrator never reads the Coder's chat transcript — only the completion report on disk and the structured return prompt that the human pastes in. The Auditor never shares context with the Coder.

---

## Step 1 — Copy The `LLM/` Folder

Copy the entire `LLM/` folder **and `ORCHESTRATOR_PROMPT.md`** from this template into your project root. (`QUICK_START.md`, `MODEL_ROUTING_GUIDE.md`, and `examples/` stay in the template — read them from there.)

You do not need every subfolder yet. Create artifacts as you reach the layer that needs them.

---

## Step 2 — Pick The Role Start File

Use exactly one role start file per fresh session:

| Role | Start file |
|---|---|
| Orchestrator | `LLM/START_ORCHESTRATOR.md` |
| Coder | `LLM/START_CODER.md` |
| Auditor | `LLM/START_AUDITOR.md` |
| Deep planner | `LLM/START_DEEP_PLANNER.md` |
| Scout (one-time, adoption) | `LLM/START_SCOUT.md` |

`LLM/START_HERE.md` is only a router.

---

## New Project Quick Start

Use this if your repo has little or no existing planning.

```text
1. Start an Orchestrator session with LLM/START_ORCHESTRATOR.md.

2. Set LLM/SPEC.md to New Project Mode.

3. Author requirements in LLM/requirements/{area}.md
   (template: LLM/REQUIREMENTS_TEMPLATE.md).

4. Author design in LLM/design/{area}.md
   (template: LLM/DESIGN_TEMPLATE.md).

5. Break design into work packages in LLM/tasks/{area}.md
   (template: LLM/TASKLIST_TEMPLATE.md).

6. Pick one WP. Write a handoff in LLM/handoffs/WP-{ID}.md
   (template: LLM/HANDOFF_TEMPLATE.md).

7. Orchestrator updates LLM/CURRENT.md to awaiting-coder.

8. Orchestrator provides the Coder prompt and updates LLM/CURRENT.md
   to awaiting-completion.

9. Run the Coder session with LLM/START_CODER.md.
   Use a **fresh window**. Close it after the Coder writes the completion
   report and return prompt.
10. Copy the Coder's `## Orchestrator Return Prompt` and paste it into
    the Orchestrator window. Do not paste the Coder's implementation
    chat into the Orchestrator window — only the return prompt.
11. If `Status: Pass` and `Ready For Audit? Yes`, the Orchestrator gives you an Auditor prompt.
    Paste it into a **fresh window**. Close it after the Auditor writes
    the verdict.
12. Copy the Auditor's verdict summary and paste it into the Orchestrator
    window. Merge/close only if the audit verdict is Pass.

13. After Pass, have the Orchestrator move the WP to `LLM/CURRENT.md#Recently Completed`, populate `CURRENT.md#Next` with the subsequent WP, and immediately write the next handoff and Coder prompt. Do not stop at idle unless the human pauses or the task list is exhausted.
```

---

## Existing Project Quick Start

Use this if your repo already has architecture docs, ADRs, an implementation plan, or similar that already function as requirements/design.

```text
1. Recommended: run the one-time Scout session first.
   Paste "Read LLM/START_SCOUT.md and follow it." into a fresh window.
   The Scout explores the repo, sets the mode and the Existing Authority
   Docs table in LLM/SPEC.md, writes LLM/design/baseline.md if you have
   no usable architecture docs, and ends with your filled Orchestrator
   kickstart. Close the Scout window after its outputs are written.

2. Start an Orchestrator session with LLM/START_ORCHESTRATOR.md
   (or with the kickstart the Scout gave you).

3. If you skipped the Scout: set LLM/SPEC.md to Existing Project Mode
   and list existing authority docs under Existing Authority Docs.
   DO NOT duplicate them under LLM/requirements/ or LLM/design/.

4. Identify the next missing layer. Usually this is tasks or handoffs.
   Create only that layer.

5. If no task list exists for the next area, create one in LLM/tasks/.
   Use LLM/TASKLIST_TEMPLATE.md.

6. Pick one small WP from the task list. Write one handoff at
   LLM/handoffs/WP-{ID}.md. Cite existing authority docs directly.

7. Orchestrator updates LLM/CURRENT.md to awaiting-coder.

8. Orchestrator provides the Coder prompt and updates LLM/CURRENT.md
   to awaiting-completion.

9. Run the Coder session with LLM/START_CODER.md in a **fresh window**.
   Close it after the Coder writes the completion report and return prompt.

10. Copy the Coder's `## Orchestrator Return Prompt` and paste it into
    the Orchestrator window. Do not paste the Coder's implementation
    chat into the Orchestrator window — only the return prompt.

11. If `Status: Pass` and `Ready For Audit? Yes`, the Orchestrator gives you an Auditor prompt.
    Paste it into a **fresh window**. Copy the Auditor's verdict summary
    back to the Orchestrator window.

12. Otherwise resolve blocker or write a follow-up handoff.

13. After Pass, the Orchestrator moves the WP to `LLM/CURRENT.md#Recently Completed`, populates `CURRENT.md#Next` with the subsequent WP, and immediately writes the next handoff and Coder prompt. Do not stop at idle.
```

The Scout session is the sanctioned intake pass: it may read broadly, but its durable output lands only in `LLM/SPEC.md`, `LLM/design/baseline.md`, and future task lists. Do not keep standing scout output in the live `LLM/` layer.

The "create the next missing layer only" rule is the single most important discipline in existing-project mode.

---

## Standard Coder Prompt

When a handoff is ready, the Orchestrator gives this prompt (canonical form — the same one defined in `LLM/ORCHESTRATOR_BOOTSTRAP.md`):

```text
Read LLM/START_CODER.md and follow it.

Active handoff:
LLM/handoffs/WP-{ID}.md

Read `LLM/handoffs/WP-{ID}.md`, then implement or stop exactly as it describes.
Before writing code, read every file listed under `Read First`.
Follow all stop conditions.
Write `LLM/completions/WP-{ID}.md` using `LLM/COMPLETION_TEMPLATE.md`.
The completion report and your final chat response must include `## Orchestrator Return Prompt`, even if you stop early, fail verification, or are not ready for audit.
```

Before giving this prompt, the Orchestrator updates `LLM/CURRENT.md` to `awaiting-completion`.

---

## Standard Auditor Prompt

After an audit-ready completion, use:

```text
Read LLM/START_AUDITOR.md and follow it.

Audit WP-{ID}.
```

Do not audit as complete unless the completion has `Status: Pass`, says `Ready For Audit? Yes`, and includes `## Orchestrator Return Prompt`.

---

## Standard Deep Planner Prompt

When the next work is too large or ambiguous to hand off safely, the Orchestrator gives you a prompt for a **fresh** Deep Planner window:

```text
Read LLM/START_DEEP_PLANNER.md and follow it.

Decompose: {tranche / feature / ambiguous area}
Authority: {up to 3 doc paths}
Produce: {task list | requirements bridge | design bridge} for Orchestrator review.
```

The Deep Planner returns the planning artifact plus the required Orchestrator state update. It never issues Coder prompts. Close the window after its output.

---

## Standard Scout Prompt (one-time, existing projects)

```text
Read LLM/START_SCOUT.md and follow it.
```

Run once at adoption, in a fresh window. The Scout ends with your filled Orchestrator kickstart; close the window after its outputs are written.

---

## Pro Tips

### Verification Commands By Stack

| Stack | Syntax check | Test |
|---|---|---|
| Node.js | `node --check file.js` | `npm test` |
| TypeScript | `tsc --noEmit` | `npm test` |
| PHP | `php -l file.php` | `vendor/bin/phpunit` |
| Laravel | `php -l file.php` | `php artisan test` |
| Python | `python -m py_compile file.py` | `pytest` |
| Go | `go build ./...` | `go test ./...` |
| Rust | `cargo check` | `cargo test` |

Pick the smallest set of commands that gives real evidence. 1–3 is usually right.

### Handoff Hygiene

- Keep handoffs tight.
- Keep `Read First` to no more than 3 priority files.
- Procedural beats declarative.
- List out-of-scope files explicitly.
- The Coder must stop if scope expands.
- The Coder must always return with `## Orchestrator Return Prompt`.

### Skill Discipline

- The `LLM/skills/` folder starts empty.
- Do not pre-create skills.
- A skill is created when the same procedural pattern is written for the third time.
- Skills are tiny, focused, and curated by the Orchestrator only.
