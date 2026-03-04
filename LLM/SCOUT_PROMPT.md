# Scout Initialization Prompt (Optional)

Use this if you are dropping BST-STRUCTURE into a **large / unfamiliar** project and you don’t know how to fill in `LLM/ORCHESTRATOR_BOOTSTRAP.md` yet.

This is intentionally **not** a full feature map. The goal is a small “baseline” that captures:
- **Navigation Anchors:** where code lives / boundaries / conventions
- **Verification Anchors:** how to build, run, and test

Hard cap: **~150–250 lines** and **5–15 anchors**. Stop once you have the structural skeleton.

---

## ⬇️ COPY FROM HERE ⬇️

You are performing an **optional baseline scout** for a software project so we can initialize `LLM/ORCHESTRATOR_BOOTSTRAP.md` without bloating global context.

### CRITICAL SCOUT RULES
1. **DO NOT map every feature.** This is a baseline skeleton only.
2. **DO NOT read deep business-logic files** unless strictly necessary to identify boundaries/conventions.
3. **HARD CAP OUTPUT:** Your baseline output must be **~150–250 lines max** and contain **5–15 anchors max**.
4. **THIS IS FOR THE ORCHESTRATOR ONLY:** Do **not** instruct the Coding LLM to read `LLM/scout/baseline.md`, and do **not** add it to "Read These Files First" in any handoff.
5. **SECRETS / SAFETY POLICY (NON-NEGOTIABLE):**
   - Never read or print `.env`, credentials, private keys, or token files.
   - If any command output includes secrets (tokens, bearer strings, key=...), redact them.
   - Prefer referencing file paths and command names over pasting large file contents.

### Your Task
Produce a baseline map with two kinds of anchors:
- **Verification Anchors:** exact commands to install deps, boot dev server, run fast tests, run full tests (if different), lint/format (if present).
- **Navigation Anchors:** primary entry points and “where changes belong” (routes, controllers, models/entities, services/actions/interactors, jobs/queues, mailers/notifications, config, migrations/schema, tests).

### Two Execution Modes (pick the one available)

#### Mode A (Preferred): You have filesystem/tool access
Use your tools to:
1) identify the framework/stack (e.g., via dependency files),
2) list top-level directories,
3) locate the canonical entry points (routes/controllers/jobs/migrations/tests),
4) infer “where code should live” conventions.

#### Mode B (Tool-agnostic): You do NOT have filesystem/tool access
Ask me (the user) for a small set of command outputs and file paths, then proceed.

Request only what you need, but usually:
- Stack/version: `php -v` + `php artisan --version` (Laravel) OR `ruby -v` + `bundle exec rails -v` (Rails)
- Routes: `php artisan route:list` OR `bundle exec rails routes` (bounded: ask me to filter/limit if huge)
- Test command(s): `php artisan test` / `vendor/bin/phpunit` OR `bundle exec rspec` / `bundle exec rails test`
- Top-level tree (depth 2–3) and the names of any “domain/service” folders used

### Deliverables (keep them small)
1) Provide the contents for a new file: `LLM/scout/baseline.md` containing:
   - Verification Anchors (exact commands)
   - Navigation Anchors (5–15 bullets)
   - “Do not touch” / safety constraints (if any)
   - Any notable conventions (short, stable only)
   - A note: "Do not add this file to handoffs / Coding LLM reading lists."
2) Provide the minimal edits needed to `LLM/ORCHESTRATOR_BOOTSTRAP.md`:
   - Fill Project Overview fields (Project/Stack/Root/Module system)
   - Add a short “Project Map (Baseline Anchors)” section that summarizes the baseline (do not paste the whole baseline)
   - Leave the “Current Features” table mostly blank or generic unless you can confidently infer 2–5 stable interfaces (routes/commands/jobs).

Stop after producing those deliverables.

## ⬆️ COPY TO HERE ⬆️
