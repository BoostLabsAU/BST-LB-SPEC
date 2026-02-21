# Quick Start — Drop This Into Any Project

## Step 1: Copy the LLM folder
Copy the entire `LLM/` folder from this template into your project root.

## Step 2: Fill in ORCHESTRATOR_BOOTSTRAP.md
Open `LLM/ORCHESTRATOR_BOOTSTRAP.md` and fill in:
- **Project Name** — e.g., "ClientPortal SaaS"
- **Stack** — e.g., "PHP 8.2, Laravel 11, MySQL 8, Tailwind CSS"
- **Root** — e.g., `C:\Projects\client-portal`
- **Module system** — e.g., "PSR-4 autoload"
- **Current Features** table — list what already exists

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
- **List exactly which files to read** — never say "read the whole project"
- **List exactly which files to modify** — never say "modify as needed"
- **Include example output** — so the Coding LLM knows what "done" looks like
- **Include verification commands** — so they can prove their work
- **Include the completion report format** — so output is always consistent
