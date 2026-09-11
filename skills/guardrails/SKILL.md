---
name: guardrails
description: Establishes security boundaries, sandboxing configurations, secret protection, and irreversible action gating to enable safe, unsupervised agent execution. Triggers on /guardrails.
---

# /guardrails

> **Principle 8**: Trust the boundaries, not the agent.  
> *"Don’t accept the false choice between letting the agent run wild on your laptop and clicking 'accept' on every single tool call. Instead, constrain the agent’s access to the files, tools, network, and credentials it actually needs."*  
> **Source**: [kiro.dev/topics/frontier-engineering/trust-the-boundaries](https://kiro.dev/topics/frontier-engineering/trust-the-boundaries/)

## Purpose & Objective
To run agents for hours or overnight with peace of mind, you must have hard boundaries that do not rely on your constant vigilance.
`/guardrails` audits and enforces deterministic guardrails:
- Gating irreversible actions (e.g. `rm -rf`, `DROP TABLE`, force pushes).
- Isolating environment credentials and shielding production secrets.
- Setting up static security analysis pre-commit.

## When to Reach for It
- Before launching unsupervised autonomous loops or continuous background agents.
- When setting up CI/CD pipeline agents or integration runners.
- Triggered by typing `/guardrails [audit | enforce | add-rule]`.

## Standalone & Synergy Characteristics
- **Standalone**: Directly inspects `.gitignore`, environment variables, git hooks, and writes safety policies into `AGENTS.md` and `.frontier/guardrails.json`.
- **Synergy with Frontier Suite**: Establishes strict safety invariants (secret shielding, destructive command blocking, git push gates) that protect the repository during unsupervised `/let-it-cook` or `/agent-complete` execution.

---

## The 4 Boundary Pillars

### 1. File System Sandboxing
- Block agent writes to sensitive directories: `.git/**`, `.github/workflows/**`, `infrastructure/prod/**`.
- Enforce that `.env*` files are strictly read-only and never included in git commits.

### 2. Command Gating (Destructive Prevention)
Deterministic prohibition of commands that cannot be undone:
- ❌ `git push --force` or `git reset --hard origin/main`
- ❌ `rm -rf /` or recursive deletion outside build caches
- ❌ `npm publish` or production deployment scripts without manual confirmation

### 3. Secret Shielding
- Scan code for regex patterns matching AWS, OpenAI, GitHub, or Stripe API keys before commits are recorded.
- Ensure all test credentials utilize local dummy mocks (e.g., `sk-test-mock-key-0000`).

### 4. Verification
Run `/guardrails audit` to generate a security posture report confirming all boundaries are locked down.
