---
name: let-it-cook
description: Executes long-running autonomous tasks (30+ minutes) with self-correction and automated test loops, freeing the developer from babysitting. Triggers on /let-it-cook.
---

# /let-it-cook

> **Principle 2**: Maximize agent time, minimize your involvement.  
> *"The constraint on your productivity is no longer typing speed; it’s how many agents you can keep meaningfully busy at once."*  
> **Source**: [kiro.dev/topics/frontier-engineering/maximize-agent-time](https://kiro.dev/topics/frontier-engineering/maximize-agent-time/)

## Purpose & Objective
Stop the 60-second micro-loop (prompt -> wait -> copy-paste error -> repeat).
`/let-it-cook` unloops the human developer. It accepts a task with built-in validation steps and runs an autonomous execution loop: writing code, running tests, reading failures, self-correcting, and converging on the solution while the developer works on something else.

## When to Reach for It
- Implementing a well-defined feature, writing unit tests for an entire module, or performing a multi-file refactor.
- When you want to kick off a task and walk away for 30 minutes, an hour, or overnight.
- Triggered by typing `/let-it-cook <task description or path to spec>`.

## Standalone & Synergy Characteristics
- **Standalone**: Accepts natural language tasks, checklist files, or specs created by `/write-intent`.
- **Synergy with Frontier Suite**: Serves as the primary autonomous execution engine of the suite, orchestrating multi-ticket batches with automated circuit breakers, test feedback, and notification dispatches.

---

## Execution Workflow

### Step 1: Pre-flight Verification Check
1. Read the task intent or ticket list.
2. Confirm the local verification command (e.g., `npm test`, `pytest`, or build command).
3. Ensure the test harness runs cleanly before making changes.

### Step 2: The Autonomous Loop
For each sub-goal:
```
┌────────────────────────────────────────┐
│ 1. Formulate change hypothesis         │
│ 2. Apply code edits                    │
│ 3. Execute validation command          │
│    ├── SUCCESS -> Commit & move to next│
│    └── FAILURE -> Read compiler/test   │
│                   diagnostics & LOOP   │
└────────────────────────────────────────┘
```
- **Rule of Self-Correction**: When an error occurs, do not ask the human what to do. Inspect the stack trace, check imports/types, fix the code, and re-run.
- **Sub-task Notification**: Upon completing each task, trigger notification if hook exists:
  ```bash
  node .frontier/scripts/notify.js --event=task_done --title="Completed: [Task Title]" --message="Passed validation tests"
  ```
- **Circuit Breaker**: If the agent attempts 5 consecutive self-corrections on the exact same error without progress, pause, record the blocker, and trigger:
  ```bash
  node .frontier/scripts/notify.js --event=circuit_breaker --title="Blocked: [Task Title]" --message="Stuck on error after 5 retries" --details="[Error snippet]"
  ```
  Then write the blocker to `.frontier/tuning-log.md` and alert the developer.

### Step 3: Completion Report
When all validation checks pass:
1. **Trigger Batch Completion Notification**:
   ```bash
   node .frontier/scripts/notify.js --event=batch_done --title="All Tasks Completed" --message="Completed all [N] tasks successfully"
   ```
2. Provide a concise summary of files changed.
3. Provide proof of test passes (test runner output snippet).
4. Offer next step: `/staff-review` to verify architectural integrity.
