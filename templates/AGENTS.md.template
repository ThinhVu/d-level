# AGENTS.md - Frontier Engineering Steering Guidelines

> **Autonomous Agent Steering Contract**: This repository follows the [Frontier Engineering](https://kiro.dev/topics/frontier-engineering) methodology. Agents onboard every session. Read this file first before taking any action.

---

## 1. Fast Feedback & Verification Commands
Always validate changes locally using these exact commands before declaring a task done:
- **Lint & Format**: `npm run lint` / `npm run format:check`
- **Typecheck**: `npm run typecheck` / `npx tsc --noEmit`
- **Unit Tests**: `npm test`
- **Integration / Invariant Tests**: `npm run test:e2e` (or boundary test suite)
- **Build**: `npm run build` (skip redundant manual build if a hot watcher is actively compiling)
- **Autonomous Verification Protocol**: For release preparation (`/let-it-ship`) or major feature refactors, generate an executable verification protocol and evidence report at `docs/verification/<version>.md` detailing machine-run test scenarios, captured evidence, and expected invariants.

> [!IMPORTANT]
> Never report completed code without executing the validation loop. If tests fail, iterate and self-correct automatically.

---

## 2. Core Architectural Invariants & Boundaries
- **Treat Implementation as Disposable**: You may refactor or completely rewrite internal logic as long as public interfaces, API contracts, and boundary invariant tests remain green.
- **Durable Decisions First**: System designs, database schemas, and external API contracts cannot be changed arbitrarily. Document significant tradeoffs in `docs/adr/`.
- **Codebase Memory**: Add descriptive code comments explaining the *reasoning* and *tradeoffs* behind complex logic so future agent sessions inherit the context.

---

## 3. Autonomous Execution Protocol (`/let-it-cook`)
When working on autonomous or long-running tasks:
1. **Define Acceptance Criteria**: Verify the "Definition of Done" and testable properties before typing code.
2. **Loop & Self-Correct**: If a test or build fails, read the compiler/runtime output, formulate a fix hypothesis, edit, and re-run. Do not stop to prompt the user unless a fundamental architectural contradiction is reached.
3. **Notification Hooks & Circuit Breaker**: If `.frontier/scripts/notify.js` exists, dispatch notifications on sub-task completion (`task_done`). If stuck on the exact same failure after 5 retries, halt via `circuit_breaker` and alert the developer.
4. **Keep Atomic Commits**: Keep changes scoped to the task; do not sprawl into unrelated modules.

---

## 4. Property-Based Testing & Correctness (`/property-test`)
- Critical business logic, parsers, and financial transformations must be verified with property-based tests across hundreds of randomized inputs to catch edge cases before shipping.

---

## 5. Safety Guardrails & Forbidden Actions
- ❌ **Never** push changes to remote git (`git push`) without explicit user review and approval.
- ❌ **Never** access, print, or commit raw `.env` secrets, API keys, or production tokens.
- ❌ **Never** execute destructive git commands (`git push --force`, `git reset --hard` on main/master).
- ❌ **Never** modify production deployment configurations without explicit user permission.
- ❌ **Never** install unverified packages or execute arbitrary external remote binaries without user confirmation.

---

## 6. Continuous Meta-Tuning
- If you make a wrong turn or encounter friction during a session, log the lesson into `.frontier/tuning-log.md` using the `/tune-up` skill so the next agent session will avoid repeating the mistake.
