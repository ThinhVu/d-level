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
`/safe-rewrite` executes a clean, fearless rewrite by first **locking the boundary invariant tests** (E2E, API contracts, property invariants). Once the boundary tests are guaranteed permanent, the agent deletes the old implementation entirely and rebuilds a modern, clean version from scratch without regression.

## When to Reach for It
- When an existing module has become too convoluted or accumulated unmanageable tech debt.
- When migrating an entire component to a new pattern (e.g., class-based to functional, callback to async/await, REST to GraphQL).
- Triggered by typing `/safe-rewrite <module or file path>`.

## Standalone & Synergy Characteristics
- **Standalone**: Automatically identifies external callers, extracts behavioral contracts into boundary tests, and performs the rewrite.
- **Synergy with Frontier Suite**: Embodies the "Treat Implementation as Disposable" principle, working closely with `/fast-loop` and `/property-test` to guarantee zero regressions.

---

## Execution Workflow

### Step 1: Lock the Boundary Contract
Before touching a single line of production code:
1. Identify all public functions, API endpoints, or exported interfaces of the target module.
2. Write or verify **Boundary Contract Tests** (`tests/contracts/<module>.contract.spec.ts`) capturing the exact required inputs, outputs, error codes, and edge-case behaviors.
3. Verify that the *existing* code passes these boundary tests.

### Step 2: Discard Old Implementation
Delete or move the old internal implementation into a temporary backup branch. Do not cling to old lines of code or nursing first drafts.

### Step 3: Rebuild Clean Implementation
1. Write the new implementation from the ground up using modern conventions, strict typing, and clean architecture.
2. Run the boundary contract tests continuously until 100% passing.
3. Run project-wide integration tests to verify zero regressions for consumers.

### Step 4: Verification Summary
Show the before/after comparison: lines of code eliminated, complexity reduced, and proof of passing boundary contracts.
