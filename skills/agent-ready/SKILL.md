---
name: agent-ready
description: Audits and refactors a codebase or module to make it agent-friendly by establishing steering files, clear module boundaries, strong typing, and persistent decision records. Triggers on /agent-ready.
---

# /agent-ready

> **Principle 3**: Build your codebase for agents.  
> *"While a new human team member onboards only once, an agent onboards every single session."*  
> **Source**: [kiro.dev/topics/frontier-engineering/build-for-agents](https://kiro.dev/topics/frontier-engineering/build-for-agents/)

## Purpose & Objective
Agents are only as productive as the codebase they operate within. `/agent-ready` audits your repository or target sub-module to remove friction points that cause agents to hallucinate, produce broken imports, or make inconsistent architectural decisions.

## When to Reach for It
- When onboarding an existing or legacy repository to AI coding agents.
- Before delegating a complex subsystem to autonomous loops.
- Triggered by typing `/agent-ready [module path or entire repo]`.

## Standalone & Synergy Characteristics
- **Standalone**: Analyzes folder architecture, exports, types, and writes module-level READMEs and architectural steering files.
- **Synergy with Frontier Suite**: Forms the "Definition of Ready" (DoR) that prepares the technical scaffolding (strict typing, fast builds, isolated module scopes, script automation) before executing `/agent-complete` (DoD) or `/let-it-cook`.

---

## Execution Workflow

### Step 1: Agent Friction Audit
The agent inspects the target directory against the **Frontier Agent-Readiness Checklist**:
1. **Module Boundaries**: Are modules isolated with clean public barrels/index files, or are there circular spaghetti imports?
2. **Type Safety**: Are types loose (`any`, implicit types) or strictly defined with compiler guarantees?
3. **Build & Error Feedback**: Do build tools report readable line numbers and actionable error messages?
4. **Agent Steering Context**: Is there an up-to-date steering file or module-level `README.md` defining conventions?

### Step 2: Brownfield Legacy Modernization (Extract AS-IS Spec)
For existing, undocumented legacy modules (inspired by Amazon Prime Video's 10-year-old service modernization):
1. **Analyze Existing Behavior**: Trace entry points, public functions, side effects, database queries, and error branches.
2. **Generate AS-IS Specification**: Create `docs/specs/<module>-as-is.md` detailing:
   - Existing API contracts & payload shapes.
   - Implicit invariants (e.g. "always deducts tax before fee calculation").
   - Known quirks, technical debt, and untested edge cases.
3. **Establish Characterization Tests**: Write regression tests capturing current behavior *before* any refactoring begins.

### Step 3: Remediate in Place
The agent offers and executes targeted improvements:
- Create or update module READMEs describing the module's public contract and dependencies.
- Add missing type definitions for core data structures.
- Generate scoped scripts for testing that specific module in isolation (so agents don't have to run the entire 10-minute monorepo test suite).

### Step 4: Record Decision Context (ADR)
Ensure that non-obvious engineering decisions are documented in code comments and lightweight Architectural Decision Records (`docs/adr/`) so subsequent agent sessions inherit the design reasoning.
