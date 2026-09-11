---
name: spike-off
description: Settle architectural and technical debates by building parallel prototype spikes with runnable benchmark evidence before committing to an approach. Triggers on /spike-off.
---

# /spike-off

> **Principle 5**: Execution is cheap. Direction is everything.  
> *"When two designs are both plausible, have the agent prototype both and compare them. Choices that used to be settled by debate can now be settled by evidence."*  
> **Source**: [kiro.dev/topics/frontier-engineering/direction-over-execution](https://kiro.dev/topics/frontier-engineering/direction-over-execution/)

## Purpose & Objective
Implementation details are cheap to change; architectural commitments and dependency choices are expensive. When facing a dilemma (e.g. *Option A vs Option B*), do not waste hours arguing in meetings or theorizing. `/spike-off` builds working code spikes for both options, runs benchmark or usability comparisons, and delivers an **evidence-based recommendation**.

## When to Reach for It
- Deciding between two third-party libraries (e.g., Zod vs Valibot, Prisma vs Drizzle, Redux Toolkit vs Zustand).
- Evaluating two system architectures (e.g., polling vs WebSocket, monolithic table vs relational joins).
- Triggered by typing `/spike-off <Option A> vs <Option B> for <Problem Context>`.

## Standalone & Synergy Characteristics
- **Standalone**: Creates isolated spike branches or scratch directories, implements realistic minimal working examples, and benchmarks them.
- **Synergy with Frontier Suite**: Resolves architectural debates before writing full system specifications in `/draft-rfc`, producing quantitative benchmarks (performance, bundle size, DX, line count) to inform decisions.

---

## Execution Workflow

### Step 1: Define Evaluation Criteria
Establish 3-4 objective metrics with the user:
- Bundle size & dependency footprint
- Execution latency & memory usage
- Ergonomics & readability (lines of code required)
- Type safety and IDE autocompletion

### Step 2: Build Parallel Working Spikes
In isolated folders or temporary git branches:
1. Implement the minimal realistic scenario using **Option A**.
2. Implement the exact same scenario using **Option B**.
3. Run identical test assertions against both implementations.

### Step 3: Comparative Evidence Report
Generate a Markdown scorecard:
```markdown
# Spike-Off: Prisma vs Drizzle for User Order Querying

| Metric | Option A: Prisma | Option B: Drizzle | Winner |
|---|---|---|---|
| Query Latency (1k rows) | 24ms | 9ms | Option B (2.6x faster) |
| Bundle Size Overhead | ~2.1 MB (engine binary) | ~45 KB | Option B |
| Migration Ergonomics | Declarative schema | SQL-first migrations | Option A |
| Type Inference Speed | Good | Instantaneous | Option B |

### Recommendation
Option B (Drizzle) is recommended for serverless latency requirements.
```

### Step 4: Cleanup
Discard the losing branch and provide an integration plan for the winner.
