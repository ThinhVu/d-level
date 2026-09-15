---
name: property-test
description: Generates property-based test suites and randomized fuzz tests directly from intent specs and acceptance criteria to verify edge-case correctness before shipping. Triggers on /property-test.
---

# /property-test

> **D-Level Teams Tenet**: Correctness by design, not just speed by default.  
> *"Property-based testing extracts testable properties directly from requirements and generates hundreds of randomized test cases probing edge cases no human would write by hand. Code is verified against intent before it ships, not after."*  
> **Source**: [kiro.dev/topics/frontier-teams](https://kiro.dev/topics/frontier-teams/)

## Purpose & Objective
Traditional unit tests only check explicit example cases that the developer remembers to write (e.g. `test("adds 2 + 2 = 4")`). AI agents can write hundreds of lines of code that pass basic example tests but fail on unexpected edge cases: unicode strings, null bytes, empty collections, integer overflow, concurrency races, or out-of-order payloads.

`/property-test` derives **mathematical and behavioral invariants** directly from your specification or acceptance criteria (`docs/intent/<feature>.md`), then generates property-based tests that barrage your code with hundreds of randomized, adversarial inputs.

## When to Reach for It
- After writing a feature or data transformation logic, before opening a pull request.
- When validating critical business logic: financial calculations, parsers, serializers, state machines, sorting/filtering, and permission boundaries.
- Triggered by typing `/property-test <target function, module, or spec path>`.

## Multi-Language Harness Support
`/property-test` automatically detects your project stack and uses the idiomatic property test framework:
- **TypeScript / JavaScript**: [`fast-check`](https://github.com/dubzzz/fast-check)
- **Python**: [`hypothesis`](https://hypothesis.readthedocs.io/)
- **Go**: `testing/quick` or [`pgregory.net/rapid`](https://github.com/flyingmutant/rapid)
- **Rust**: [`proptest`](https://github.com/proptest-rs/proptest)
- **Java / Kotlin**: [`jqwik`](https://jqwik.net/)

---

## 4 Universal Property Patterns

When analyzing your intent spec, `/property-test` maps acceptance criteria into one of 4 property archetypes:

### 1. Invariants (Things that must ALWAYS be true)
- *Example*: "The balance of all accounts combined must remain constant after any sequence of transfers."
- *Example*: "The output list size must never exceed the input list size when filtering."

### 2. Round-Trip (Serialization / Deserialization symmetry)
- *Example*: `deserialize(serialize(payload)) == payload` for all arbitrary JSON/Binary structures.
- *Example*: Compressing and decompressing any arbitrary byte sequence yields the original bytes.

### 3. Idempotency (Applying operation multiple times has same effect as once)
- *Example*: `sanitize(sanitize(userInput)) == sanitize(userInput)`
- *Example*: Running database sync/migration twice does not duplicate records.

### 4. Test Oracle / Differential Testing (Comparison against baseline)
- *Example*: Optimizing an algorithm or rewriting a module: the new high-performance implementation must match the original naive implementation across 1,000 random inputs.

---

## Execution Workflow

### Step 1: Extract Invariants from Spec
Read the feature code and corresponding intent doc (`docs/intent/*.md`). Formulate 2–4 fundamental properties that must hold true across any valid input.

### Step 2: Install & Scaffold Property Framework
If the framework is not yet installed in `devDependencies`, install it using the project's package manager:
```bash
# TypeScript/JS
npm install --save-dev fast-check
# Python
poetry add --group dev hypothesis # or pip install hypothesis
```

### Step 3: Generate Generative Test Suite
Write tests in the project's test directory (e.g. `tests/properties/<module>.property.test.ts`).
Example using `fast-check`:
```typescript
import fc from 'fast-check';
import { calculateTaxDiscount } from './order-calculator';

test('Order total is always non-negative and never exceeds item sum', () => {
  fc.assert(
    fc.property(
      fc.array(fc.record({ price: fc.float({ min: 0.01, max: 10000 }), qty: fc.integer({ min: 1, max: 100 }) })),
      fc.float({ min: 0, max: 0.5 }), // discount 0-50%
      (items, discountRate) => {
        const result = calculateTaxDiscount(items, discountRate);
        expect(result.finalTotal).toBeGreaterThanOrEqual(0);
        expect(result.discountApplied).toBeLessThanOrEqual(result.subtotal);
      }
    ),
    { numRuns: 500 }
  );
});
```

### Step 4: Run & Auto-Shrink Edge Cases
Run the property suite:
- When a failure is found, the framework automatically **shrinks** the failing input to the simplest minimal reproduction (e.g., `""`, `0`, or `[-1]`).
- Fix the bug in the implementation and re-run until all 500+ randomized iterations pass.
