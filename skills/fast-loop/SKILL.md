---
name: fast-loop
description: Builds or optimizes fast, local automated feedback harnesses (sub-10s unit tests, linters, headless browser checks, service mocks) enabling agents to self-validate in tight loops. Triggers on /fast-loop.
---

# /fast-loop

> **Principle 4**: Give agents a fast feedback loop.  
> *"If your agent can’t run tests locally and self-correct before you ever look at the code, you’re the bottleneck."*  
> **Source**: [kiro.dev/topics/frontier-engineering/fast-feedback-loop](https://kiro.dev/topics/frontier-engineering/fast-feedback-loop/)

## Purpose & Objective
Code generation is instantaneous; waiting 5 minutes for CI or a slow test runner kills autonomous velocity. `/fast-loop` builds, verifies, and optimizes local verification harnesses so that an agent can validate any file edit in **under 10 seconds**.

## When to Reach for It
- When test suites take too long (>30s) for rapid agent iterations.
- When working on UI changes that need automated headless browser validation.
- When backend code requires local service mocks (DB, Redis, external APIs) to test without external network latency.
- Triggered by typing `/fast-loop [component or test target]`.

## Standalone & Synergy Characteristics
- **Standalone**: Analyzes test configurations (Vitest, Jest, Pytest, Playwright) and sets up fast targeted watch/run scripts and mocks.
- **Synergy with D-Level Suite**: Provides the blazing-fast (<10s) testing foundation that powers the autonomous self-correcting execution in `/let-it-cook` and `/agent-complete`.

---

## Execution Workflow

### Step 1: Benchmark Existing Feedback Loop
1. Measure execution time of current lint and test commands.
2. Identify bottlenecks (e.g. database spinup, full codebase bundling, slow unmocked external HTTP calls).

### Step 2: Implement Targeted Fast-Harness
Depending on the project stack:
- **Scoped Test Runner**: Create targeted scripts like `npm run test:fast -- path/to/file` with in-memory SQLite/mocks rather than full container setups.
- **Visual UI Verification**: Provide headless browser scripts (e.g., Playwright/Puppeteer script or dev server check) so agents can verify DOM state and render without human visual inspection.
- **Property-Based Testing**: Add fast invariant assertions (e.g. `fast-check` in TS, `hypothesis` in Python) that generate 100 randomized inputs to catch edge cases in <1s.

### Step 3: Register in Steering Files
Save the newly configured fast command in `AGENTS.md` and `.d-level/config.json` so all subsequent agent sessions immediately utilize the optimized loop.
