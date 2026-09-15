---
name: ask-d-level
description: Navigation and routing assistant for the D-Level workflow. Answers questions about which skill to use for any given task. Triggers on /ask-d-level.
---

# /ask-d-level

> **Tenet**: Navigating autonomous workflows and engineering leverage.  
> **Source**: [kiro.dev/topics/frontier-engineering](https://kiro.dev/topics/frontier-engineering)

## Purpose & Objective
This skill acts as your **chief-of-staff navigator**. Tell it what you want to achieve, and it tells you the exact sequence of skills to invoke, why, and what inputs to provide.

## When to Reach for It
- Whenever you are unsure which skill applies to your current situation.
- When planning a multi-phase feature, refactor, or incident investigation.
- Triggered by typing `/ask-d-level <your query>`.

## Standalone & Synergy Characteristics
- **Standalone**: Routes intelligently across all D-Level skills (from spec drafting to autonomous loops, release orchestration, and incident postmortems).
- **Synergy with D-Level Suite**: Acts as the central dispatcher and router for the entire D-Level suite, ensuring agents and developers select the optimal workflow for their goals.

---

## 🔄 The 4-Phase D-Level Teams Lifecycle

When building production features, follow the canonical D-Level Teams loop:
1. **Capture Intent**: `/write-intent` (turn natural language into structured acceptance criteria).
2. **Design Architecture**: `/draft-rfc` & `/spike-off` (system design, dependency-sequenced task plan).
3. **Build to Context**: `/let-it-cook` & `/agent-complete` (unattended autonomous build with automated feedback).
4. **Verify & Ship**: `/property-test` (randomized fuzzing), `/staff-review` (architectural diff audit), and `/let-it-ship` (automated changelog & release notes).

---

## Routing Matrix (Quick Reference)

| If your goal is... | Recommended Skill |
|---|---|
| Initialize or configure this repo for AI agents | ➡️ `/setup-d-level` |
| Clarify vague requirements & definition of "Done" | ➡️ `/write-intent` |
| Launch a 30m+ autonomous task without babysitting | ➡️ `/let-it-cook` |
| Make legacy code agent-friendly or extract AS-IS spec | ➡️ `/agent-ready` |
| Speed up local test & feedback cycles to <10s | ➡️ `/fast-loop` |
| Settle an architectural debate between 2 options | ➡️ `/spike-off` |
| Rewrite a module with 100% contract preservation (API/SDK) | ➡️ `/safe-rewrite` |
| Modernize internal subsystem, migrate callers & prune dead code | ➡️ `/smart-rewrite` |
| End-to-end enterprise subsystem modernization & parity | ➡️ `/agent-complete` |
| Fuzz edge-case correctness with randomized properties | ➡️ `/property-test` |
| Deep review on code diff before merging | ➡️ `/staff-review` |
| Restrict sandbox, permissions, and guard secrets | ➡️ `/guardrails` |
| Write an Architecture RFC or Design Doc | ➡️ `/draft-rfc` |
| Investigate a production bug or write a postmortem | ➡️ `/postmortem` |
| Create a step-by-step operational runbook | ➡️ `/runbook` |
| Cut a release, compile changelog & release notes | ➡️ `/let-it-ship` |
| Turn an agent mistake into a permanent rule | ➡️ `/tune-up` |

---

## Example Usage
```
User: /ask-d-level We want to migrate our database from MongoDB to Postgres, but the codebase is huge and we aren't sure if Prisma or Drizzle is faster.

Agent Response:
Here is your D-Level game plan:
1. Run `/spike-off Prisma vs Drizzle for user order querying` to build two quick prototype spikes with benchmark evidence.
2. Once the direction is chosen, run `/draft-rfc Database Migration from Mongo to Postgres` to formalize the migration strategy and rollback plan.
3. Lock your boundary invariant tests with `/safe-rewrite` so data layer changes never break external API contracts.
4. Launch the migration batch autonomously with `/let-it-cook`.
```
