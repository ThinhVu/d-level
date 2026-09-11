---
name: agent-complete
description: Drives end-to-end modernization of legacy enterprise subsystems into AI-first architectures with reverse-engineering audits, contract locking, tracer-bullet decomposition, and self-verifying parity loops. Triggers on /agent-complete.
---

# /agent-complete

> **The Sovereign Law of Legacy Modernization**  
> *"You cannot safely modernize what you have not exhaustively audited. In a legacy system, 80% of the mission-critical business logic lies in latent edge cases, side effects, and unwritten invariants. Mockups and happy paths are unacceptable."*

## Purpose & Objective
Where `/agent-ready` establishes the **Definition of Ready (DoR)** by scaffolding folders, types, and build scripts, `/agent-complete` delivers the **Definition of Done (DoD)** by autonomously modernizing an entire legacy subsystem into an AI-first, decoupled architecture.

Refactoring enterprise legacy code (monoliths, untyped frameworks, tightly coupled submodules) into an AI-first architecture fails when agents hallucinate simplifications or rush into coding without understanding existing side-effects.

`/agent-complete` is the **industrial-grade modernization orchestrator**. It replaces guesswork with a **4-Phase Gated Pipeline**:
1. **Phase 1: Deep Reverse-Engineering Audit** – Exhaustive inspection of the legacy code; zero new code is generated until all 5 logic pillars are mapped.
2. **Phase 2: Invariant Specification & Tracer-Bullet Slices** – Formal contracts, schema definitions, and dependency-sequenced vertical tickets.
3. **Phase 3: Autonomous Parity Loop** – Contract-first test creation, target implementation, self-correction, and circuit breakers with notification hooks.
4. **Phase 4: Parity Verification & Staff Review** – 100% behavioral parity audit against the legacy source and autonomous verification protocol compilation.

---

## Invocation Syntax

```bash
/agent-complete <sourcePath> to <targetPath> [--frameworks=<list>]
```

### Examples:
```bash
# Modernize legacy billing module into a modern decoupled domain service
/agent-complete src/legacy/billing to src/services/billing --frameworks Fastify,TypeScript,Zod

# Decompose untyped backend handlers into a typed modular service
/agent-complete legacy/api/orders to src/domains/orders --frameworks Express,TypeScript,Zod
```

You can also specify parameters via a side-car file `.frontier/modernize.json`:
```json
{
  "sourcePath": "src/legacy/subsystem",
  "targetPath": "src/modern/subsystem",
  "frameworks": ["TypeScript", "Fastify", "Zod"],
  "readOnlySources": ["src/legacy/**"]
}
```

---

## The 4-Phase Execution Pipeline

```text
┌────────────────────────────────────────────────────────┐
│ Phase 1: Deep Reverse-Engineering Audit                │
│ (Inspect legacy code, map all 5 pillars of logic)      │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 2: Invariant Spec & Tracer-Bullet Slices         │
│ (Document schemas, transitions, and granular tickets)  │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 3: Autonomous Parity Loop                        │
│ (Boundary Contract Test → Implement → Self-Correct)    │
└──────────────────────────┬─────────────────────────────┘
                           ▼
┌────────────────────────────────────────────────────────┐
│ Phase 4: Parity Verification & Staff Review            │
│ (100% behavioral parity audit against legacy source)   │
└────────────────────────────────────────────────────────┘
```

---

### Phase 1: Deep Reverse-Engineering Audit (Zero Code Generation)

Before writing or modifying a single file in `{{targetPath}}`, the agent **must** read and trace every relevant legacy file in `{{sourcePath}}` and extract the **5 Pillars of Legacy Logic**:

1. **API Surface & Transport**: Routes, HTTP verbs, query parameters, headers, session cookies, status codes, and exact error responses.
2. **State Machines & Lifecycles**: Order/user/record states, allowed transitions, guard conditions, timeouts, and automated status transitions.
3. **Calculations, Invariants & Business Rules**: Pricing formulas, discounts, tax computation order, idempotency checks, token formats, and retry logic.
4. **Side Effects & External Communications**: Database mutations, webhook triggers, push notifications, queue dispatches, and third-party integrations (payment providers, messaging services, email/SMS gateways).
5. **Edge Cases & Error Handling**: Missing records, negative inputs, null values, concurrency race conditions, and legacy fallback defaults.

> 🛑 **Gate 1 Enforcement**: The agent writes its findings to `docs/spec/<subsystem>-deep-spec.md`. The pipeline **cannot proceed** to Phase 2 until this specification is verified on disk.

---

### Phase 2: Invariant Specification & Tracer-Bullet Slices

Transform the audit findings into machine-executable contracts and vertical tickets:

1. **Strict Invariant Schemas**: Define validation schemas (e.g. Zod, Pydantic, JSON Schema) for all requests, responses, database entities, and event payloads.
2. **Tracer-Bullet Dependency Slices**: Decompose the migration into sequenced, end-to-end vertical slices (Schema ➔ Data Access ➔ Business Logic ➔ Transport API ➔ Contract Tests). Document these tickets in `docs/intent/migration-tickets.md`.
3. **Boundary Invariant Checklists**: Each ticket must specify:
   - Path to its future contract test: `tests/contracts/<ticket>.contract.spec.ts`.
   - Exact input/output assertions and status codes.
   - Forbidden dependencies (e.g. zero imports from legacy runtime).

---

### Phase 3: Autonomous Parity Loop

The agent processes the tickets sequentially using the autonomous loop:

```text
For each ticket in migration-tickets.md:
  1. Write Boundary Contract Test FIRST (captures required behavior & invariants).
  2. Implement target service/component in {{targetPath}} following modern frameworks.
  3. Execute verification command (npm test / pytest / cargo test).
  4. If test fails: Read diagnostics, self-correct, and re-run (up to 5 attempts).
  5. If stuck on same failure 5 times: Trigger Circuit Breaker:
     node .frontier/scripts/notify.js --event=circuit_breaker --title="Blocked: [Ticket Title]"
  6. If passed: Trigger sub-task notification:
     node .frontier/scripts/notify.js --event=task_done --title="Completed: [Ticket Title]"
  7. Commit atomically and advance to next ticket.
```

---

### Phase 4: Parity Verification & Staff Review

Once all tickets are implemented:

1. **Behavioral Parity Audit**: Cross-reference the new implementation against the Phase 1 audit matrix in `docs/spec/<subsystem>-deep-spec.md`. Confirm zero missed side effects, unhandled mutations, or omitted fallback branches.
2. **Adversarial Fuzzing**: Run `/property-test` against newly created domain calculation engines and data transformers to ensure no latent edge-case regressions.
3. **Staff Review Gate**: Run `/staff-review` across the complete diff. Check architectural isolation, secret protection, and error boundaries.
4. **Generate Autonomous Verification Protocol & Evidence**:
   Create `docs/verification/<subsystem>-protocol.md` (using `templates/verification-protocol.md.template`). The agent defines machine-actionable scenarios and captures evidence artifacts (traces, screenshot captures, response payloads) for human governance sign-off.
5. **Safety Gate**: Never push modernized branches to remote git (`git push`) without explicit user review and approval.
6. **Batch Complete Notification**:
   ```bash
   node .frontier/scripts/notify.js --event=batch_done --title="Modernization Completed" --message="All tickets passed behavioral parity"
   ```
