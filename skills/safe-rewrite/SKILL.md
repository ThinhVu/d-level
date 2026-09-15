---
name: safe-rewrite
description: Safely replaces or rewrites an entire module or subsystem by locking down external boundary contracts and invariant tests first, eliminating sunk-cost hesitation. Triggers on /safe-rewrite.
---

# /safe-rewrite

> **Principle 6**: Treat code as disposable.  
> *"Throw away code that isn’t serving you. The agent can build the next version before the end of the day. Your tests at the boundaries are the exception."*  
> **Source**: [kiro.dev/topics/frontier-engineering/code-is-disposable](https://kiro.dev/topics/frontier-engineering/code-is-disposable/)

## Purpose & Objective
Legacy codebases accumulate messy, tangled code because developers fear breaking things if they rewrite it from scratch. Sunk cost makes teams patch bad code rather than replacing it.
`/safe-rewrite` executes a clean, fearless **Black-Box Rewrite** where the external contract is strictly immutable. Once boundary contract tests are permanently locked, the agent deletes the old implementation entirely and rebuilds a modern, clean version from scratch with **zero breaking changes** to consumers.

## When to Reach for It
- **Public APIs, SDKs, Shared Libraries, & Core Utilities**: When consumers live outside your direct control (or across multiple repositories) and the external API surface **must remain 100% backward-compatible**.
- When an existing module has become too convoluted or accumulated unmanageable tech debt, but its public interface is sound and must not change.
- When migrating internal mechanics (e.g. callback to async/await, imperative loop to pipeline, canvas to webgl) without altering caller signatures.
- **Note**: If you want to migrate callers to a new unified pipeline and prune obsolete entry points/scaffolding, use `/smart-rewrite` instead.
- Triggered by typing `/safe-rewrite <module or file path>`.

## Standalone & Synergy Characteristics
- **Standalone**: Identifies public API contracts, extracts behavioral contracts into boundary tests, and performs the black-box rewrite.
- **Synergy with D-Level Suite**: Embodies the "Treat Implementation as Disposable" principle, working closely with `/fast-loop` and `/property-test` to guarantee zero regressions.

---

## Execution Workflow

### Step 1: Lock the Boundary Contract
Before touching a single line of production code:
1. Identify all public functions, API endpoints, and exported interfaces of the target module.
2. Write or verify **Boundary Contract Tests** (`tests/contracts/<module>.contract.spec.ts`) capturing exact required inputs, outputs, error codes, and edge-case behaviors.
3. Verify that the *existing* code passes these boundary tests.

### Step 2: Discard Old Implementation
Delete or move the old internal implementation into a temporary backup branch. Do not cling to old lines of code or nurse first drafts.

### Step 3: Rebuild Clean Implementation
1. Write the new implementation from the ground up using clean architecture and strict typing, while strictly matching the host codebase's established style and syntax conventions.
2. Ensure every exported symbol in the locked boundary contract behaves identically to specification.
3. Run the boundary contract tests continuously until 100% passing.
4. Run project-wide integration tests to verify zero regressions for consumers.

### Step 4: Verification Summary
Show the before/after comparison: lines of code eliminated, performance improvements, and proof of passing boundary contracts.

---

## Formatting Guardrails: Zero Cosmetic Churn

Rewriting a module does **not** grant license to impose unsolicited formatting opinions or "textbook perfection". Unnecessary stylistic churn creates noisy diffs, obscures architectural intent, and frustrates code reviewers.

| Anti-Pattern | Why It Is Harmful | Required Behavior |
| :--- | :--- | :--- |
| **Unsolicited "Perfection"** (e.g., auto-inserting semicolons, swapping quote styles, adding/removing trailing commas, altering bracket spacing) | Injects cosmetic changes that distract human reviewers and pollute git blame history. | **Mirror the Host Codebase**: Inspect existing files, linters, or formatter configs (`.editorconfig`, `.prettierrc`, ESLint, Ruff, etc.). If the project omits semicolons or uses single quotes, adhere strictly to that convention. |
| **Format Creep & Diff Bloat** | Blindly reformatting untouched lines, surrounding functions, or adjacent imports. | **Keep Diffs Surgical**: Only touch what is directly necessary for the rewritten module. Never re-indent, re-space, or "prettify" code outside the rewrite scope. |
| **Stylistic Opinion Imposition** | Overriding repository-specific conventions with generic AI default preferences. | Respect established repository idioms across all languages (naming, punctuation, indentation, casing, spacing). Never reformat for subjective aesthetic preference. |
