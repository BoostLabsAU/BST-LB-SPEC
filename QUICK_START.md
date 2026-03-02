# Quick Start — Drop This Into Any Project

## Step 0: Pick your LLMs
You need two models — an Orchestrator and a Coder. See [evals/LLM_RECOMMENDATIONS.md](evals/LLM_RECOMMENDATIONS.md) for our tested pairs, or use the main recommendation: **GPT-5.2** (Orchestrator) + **Codex 5.3** (Coder). Opus 4.6 is also a strong Orchestrator swap-in (especially for Laravel-heavy repos).

## Step 1: Copy the LLM folder
Copy the entire `LLM/` folder from this template into your project root.

## Step 2: Fill in ORCHESTRATOR_BOOTSTRAP.md
Open `LLM/ORCHESTRATOR_BOOTSTRAP.md` and fill in:
- **Project Name** — e.g., "ClientPortal SaaS"
- **Stack** — e.g., "PHP 8.2, Laravel 11, MySQL 8, Tailwind CSS"
- **Root** — e.g., `C:\Projects\client-portal`
- **Module system** — e.g., "PSR-4 autoload"
- **Current Features** table — list what already exists

*(Note: The `LLM/docs/RULES.md` file is included in this template. The Orchestrator will automatically append to it if you run into repeated errors. You do not need to write it now).*

## Step 3: Bootstrap the Orchestrator
Paste this into your Orchestrator LLM (first conversation):

```
You are the Orchestrator LLM for this project. Read the file
`LLM/ORCHESTRATOR_BOOTSTRAP.md` to understand the project structure,
existing features, and your workflow. Then ask me what I'd like to work on.
```

## Step 4: Start building
Describe your first feature. The Orchestrator will:
1. Ask clarifying questions
2. Write a context doc + handoff prompt
3. Give you the prompt for the Coding LLM

## Step 5: The Loop
```
You describe feature → Orchestrator writes handoff →
You paste into Coding LLM → Coding LLM implements →
You paste audit prompt → Orchestrator reviews code →
If issues: generates follow-up prompt →
If passed: updates docs & asks "What next?"
```

---

## Pro Tips

### For PHP/Laravel projects, the Orchestrator Bootstrap should list:
- Routes (web.php, api.php)
- Models and their relationships
- Middleware
- Key service classes
- Database migrations
- Config files

### Verification commands by stack:
| Stack | Syntax Check | Test |
|-------|-------------|------|
| Node.js | `node --check file.js` | `npm test` |
| PHP | `php -l file.php` | `php artisan test` |
| Python | `python -m py_compile file.py` | `pytest` |
| Go | `go build ./...` | `go test ./...` |

### Handoff rules of thumb:
- **Keep it tight** — Handoffs should be highly focused. Avoid big architecture summaries.
- **Provide Procedural Guidance** — Don't just declare what you want; provide step-by-step instructions and edge cases.
- **List exactly where to start reading** — but allow targeted exploration (e.g., "Start here. Explore only what's necessary; if blocked, pause and ask").
- **Include verification commands** — so they can prove their work.
- **Enforce structured completion reports** — pass/fail, commands run, extra files explored.

### Skills (emerge over time):
- The `LLM/skills/` folder starts empty — don't pre-create skills
- The Orchestrator will extract common patterns into skills after seeing them 3+ times
- Skills keep handoffs DRY without adding global context overhead
- Use skills selectively — prefer 0–3 relevant skills per handoff; skip them for routine tasks.
