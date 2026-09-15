---
name: smart-rewrite
description: Intelligently refactors, unifies, and modernizes application subsystems by migrating callers to canonical pipelines, tearing down transitional scaffolding, and aggressively eliminating dead code. Triggers on /smart-rewrite.
---

# /smart-rewrite

> **Deep Modules & Clean Seams**:  
> *"The best modules are those that provide powerful functionality through a simple interface. When unifying architectures, transitional wrappers are temporary scaffolding—tear them down the moment callers migrate."*  
> **Source**: Inspired by *A Philosophy of Software Design* (John Ousterhout) & Frontier Clean Architecture.

---

## Purpose & Objective

While `/safe-rewrite` executes a strict **black-box rewrite** that preserves 100% of external function signatures (ideal for public APIs, SDKs, and shared libraries), real-world applications often need something fundamentally different: **Architectural Modernization & Clean Code**.

In monolithic or internal application codebases, subsystems accumulate messy wrappers, duplicated entry points, and circular dependencies over time. A simple "safe rewrite" falls into **The Scaffolding Trap**—preserving obsolete wrappers and writing superficial compatibility tests for functions that no longer have real callers.

`/smart-rewrite` coordinates an end-to-end modernization:
1. Designs a clean, unified canonical pipeline (Deep Module).
2. Migrates all production callers to the new pipeline.
3. Performs a **Post-Migration Seam Sweep** to tear down transitional scaffolding and eradicate dead code.
4. Leaves the codebase cleaner, leaner, and free of cargo-cult backward compatibility wrappers.

---

## When to Reach for It

- **Internal Subsystems & Monolith Modules**: When you own both the callers and the module being refactored (Closed-World scope).
- **Architecture Unification**: When consolidating fragmented legacy patterns (e.g. 5 different direct driver calls unified into a single `printHandler` dispatcher).
- **Interface Simplification**: When slimming down a bloated module surface with dozens of ad-hoc exports into 2-3 clean, high-leverage entry points.
- **Triggered by typing `/smart-rewrite <module or subsystem>`**.

---

## The Two Rewriting Philosophies

| Dimension | `/safe-rewrite` (Black-Box) | `/smart-rewrite` (Architectural Modernization) |
| :--- | :--- | :--- |
| **Primary Domain** | Public APIs, SDKs, npm packages, Open Boundaries | Internal application code, Monoliths, Closed Boundaries |
| **Caller Scope** | External / Unknown callers outside repository | Internal callers owned 100% within the repository |
| **External Interface** | **Immutable (100% preserved)** | **Evolves (Retires old seams, unifies pipeline)** |
| **Old Wrappers** | Retained to prevent breaking changes | **Torn down as temporary scaffolding** |
| **Success Metric** | Zero signature changes | Maximum code deletion, clean seams, zero dead wrappers |

---

## Execution Workflow

### Step 1: Pre-Audit Callers & Identify Migration Seams
Before touching code:
1. **Grep the codebase** for all references to each exported symbol of the target module.
2. **Map the Caller Ecosystem**:
   - Which callers are active?
   - What are their actual business requirements (e.g., "needs to print a kitchen ticket", not "needs to call this specific legacy wrapper")?
3. **Classify Interfaces**:
   - **Canonical Target**: The new, unified interface you will expose (Deep Module).
   - **Retiring Interfaces / Scaffolding**: Old functions that will be dismantled once callers are repointed.
   - **Dead Code**: Any function with 0 callers already. Delete immediately.

### Step 2: Lock the Canonical Domain Invariants
1. Write **Domain Invariant Tests** for the core business logic (calculations, formatting, opcodes, state transitions).
2. **DO NOT** write contract tests for retiring scaffolding functions. Writing tests like `expect(typeof legacyFn).toBe('function')` is strictly forbidden—it enshrines dead code into the test suite.

### Step 3: Implement Clean Canonical Pipeline
1. Rebuild the module around the clean canonical interface (e.g. standardizing on `makePrintData()` and `print()`).
2. Adhere to the **Deep Module Principle**: simple interface, deep functionality.
3. Export **ONLY** the minimal required canonical symbols.

### Step 4: Migrate Production Callers
1. Repoint all consumers (e.g. `order/index.js`, controllers, hooks) to the new canonical pipeline.
2. Verify with end-to-end integration tests that all real-world flows (UI triggers, queues, background jobs) operate without regression.

### Step 5: Post-Migration Seam Sweep (Tear Down Scaffolding)
> ⚠️ **CRITICAL STEP**: The definitive differentiator of `/smart-rewrite`.

Once Step 4 caller migration is complete:
1. **Re-audit the codebase**: Grep again for every old function, alias, and wrapper.
2. **Tear down scaffolding**:
   - If a wrapper was kept temporarily during migration, **DELETE IT NOW**.
   - Do NOT leave backward-compatibility pass-throughs (e.g., do not keep an old wrapper calling the new handler).
   - Delete any temporary tests asserting old function signatures.
3. Verify zero circular dependencies remain between caller and callee.

### Step 6: Verification & Complexity Reduction Summary
Produce a clean before/after report:
- **Callers Migrated**: List of consumers repointed to the canonical pipeline.
- **Dead Code & Scaffolding Eliminated**: Exact functions, lines of code, and exports permanently removed.
- **Interface Surface Reduction**: Before/after count of exported symbols.
- **Test Parity**: Verification that canonical integration tests and domain invariant tests pass 100%.

---

## Anti-Patterns & Guardrails

| Anti-Pattern | Why It Is Harmful | Required Behavior |
| :--- | :--- | :--- |
| **The Scaffolding Trap** | Keeping transitional wrappers alive after consumers have already been repointed to the new pipeline. | Execute **Step 5 Post-Migration Seam Sweep**. When callers migrate, the old wrapper is garbage—delete it. |
| **Cargo-Cult Compatibility** | Fearing to delete internal functions "just in case someone needs it in 2 years". | If grep reveals 0 callers in a closed-world repo, **delete it immediately**. |
| **Test Suite Pollution** | Writing tests like `expect(typeof oldWrapper).toBe('function')` to fake "100% backward compatibility". | Never test dead scaffolding. Tests must assert domain invariants, not enshrine transitional wrappers. |
| **Circular Dispatching** | Old wrapper calls new dispatcher, which requires old module to format data. | Sever the loop. Layout/worker modules only expose pure data/renderer hooks; the dispatcher controls the flow. |
