---
name: setup-frontier-engineering
description: Run-once initialization skill for Frontier Engineering principles. Configures steering files (AGENTS.md), fast feedback harnesses, boundary guardrails, and tuning logs. Triggers on /setup-frontier-engineering.
---

# /setup-frontier-engineering

> **Tenet**: Building the setup that builds the software.  
> **Source**: [kiro.dev/topics/frontier-engineering](https://kiro.dev/topics/frontier-engineering)

## Purpose & Objective
This is the **run-once setup skill** for a repository. It analyzes the workspace, discovers project conventions and testing harnesses, and writes persistent steering files (`AGENTS.md`, `.frontier/`, `docs/`) so all subsequent agent sessions run with maximum autonomy and safety.

## When to Reach for It
- Immediately after installing the Frontier Engineering skill suite into a new or existing repository.
- Triggered manually by typing `/setup-frontier-engineering`.

## Standalone & Synergy Characteristics
- **Standalone**: Operates 100% independently without any external dependencies. Sets up `AGENTS.md` and `.frontier/` from scratch.
- **Universal Compatibility**: If existing agent instruction files (e.g. `CLAUDE.md`, `.cursorrules`, `.windsurfrules`) already exist, it preserves and respects them, cleanly integrating the Frontier Engineering Autonomous Protocol without overwriting existing team configurations.

---

## Execution Workflow

### Step 1: Scan & Detect Environment
The agent examines the current repository:
1. Identify languages, frameworks, package managers (`package.json`, `Cargo.toml`, `go.mod`, `pyproject.toml`, etc.).
2. Locate existing testing tools (Jest, Vitest, Pytest, Go test, Cargo test) and linting setups (ESLint, Prettier, Ruff, Biome).
3. Check for existing steering files (`CLAUDE.md`, `AGENTS.md`, `.cursorrules`).

### Step 2: Propose Configuration to User
Present a concise summary to the user:
- **Detected Test Command**: (e.g., `npm test`, `pytest`)
- **Detected Lint/Format Command**: (e.g., `npm run lint`)
- **Detected Typecheck Command**: (e.g., `npm run typecheck` or `tsc --noEmit`)
- **Protected Files**: (e.g., `.env*`, `deploy/**`, migrations)

Ask the user: *"Are these verification commands and boundaries accurate for your workflow?"*

### Step 3: Write Persistent Steering Artifacts
Once confirmed:
1. Create or enrich `AGENTS.md` at the project root using `templates/AGENTS.md.template`.
2. Create `.frontier/config.json` with repository metadata and test scripts.
3. Create `.frontier/scripts/notify.js` using `templates/notify.js.template` for autonomous notification hooks.
4. Create `.frontier/notifications.json` using `templates/notifications.json.template` (add to `.gitignore` to keep webhooks private).
5. Create `.frontier/tuning-log.md` using `templates/tuning-log.md.template` to capture ongoing meta-tuning.

### Step 4: Next Steps
Print a friendly completion checklist:
- Run `/agent-ready` to audit boundaries or extract AS-IS specs from legacy modules.
- Run `/fast-loop` to verify your local testing harness runs in under 10 seconds.
- Run `/property-test` to scaffold property-based tests for critical business logic.
- Run `/ask-frontier` whenever you want routing advice on which skill to reach for.
