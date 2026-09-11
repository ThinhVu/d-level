---
name: write-intent
description: Clarifies and documents architectural intent, definition of done, acceptance criteria, and edge cases before code is generated. Triggers on /write-intent.
---

# /write-intent

> **Principle 1**: You are the architect, not the typist.  
> *"The job is no longer writing code — it is writing clear intent and verifying the output meets it."*  
> **Source**: [kiro.dev/topics/frontier-engineering/architect-not-typist](https://kiro.dev/topics/frontier-engineering/architect-not-typist/)

## Purpose & Objective
This skill stops the "vague prompt -> bad code -> rollback" cycle. It takes a raw idea or loose request and extracts **precise architectural intent**:
- What does "Done" look like?
- Exact acceptance criteria & invariants.
- Critical edge cases that must be handled.
- How will correctness be verified without human guesswork?

## When to Reach for It
- Before asking an agent to write, refactor, or fix any non-trivial feature.
- Whenever you notice yourself writing a prompt like *"Add auth to this API"* or *"Improve checkout performance"*.
- Triggered by typing `/write-intent <feature or task description>`.

## Standalone & Synergy Characteristics
- **Standalone**: Directly drafts an Intent Specification (`docs/intent/<task-slug>.md`) with verification checklists ready for `/let-it-cook`.
- **Synergy with Frontier Suite**: Acts as the essential first stage of the Frontier Teams lifecycle, establishing unambiguous, machine-verifiable acceptance criteria so `/let-it-cook` can run completely unsupervised.

---

## Execution Workflow

### Step 1: Deconstruct Intent
Ask the critical architectural questions:
1. **Behavioral Scope**: What exact endpoints, components, or state changes are included? What is explicitly out of scope?
2. **Edge Cases**: Token expiration, network timeouts, duplicate submissions, null or corrupted payloads, concurrency race conditions.
3. **Verification Method**: How will the agent verify this? (e.g. Unit test command, curl script, mock response).

### Step 2: Output Intent Specification
Generate a clean, unambiguous spec file:
```markdown
# Intent Spec: [Feature Title]

## 1. Definition of Done
- [ ] Endpoint `/api/v1/orders` requires `ORDER_WRITER` role.
- [ ] Returns `401 Unauthorized` on missing token, `403 Forbidden` on role mismatch.
- [ ] Emits `OrderCreatedEvent` to Kafka with payload schema version 2.

## 2. Invariants & Constraints
- Must not introduce any new external npm dependencies.
- Latency p95 must remain < 50ms under local mock test.

## 3. Verification Commands
- `npm test tests/auth/order-roles.spec.ts`
```

### Step 3: Handoff
Ask the user for confirmation: *"Does this intent capture the complete picture? Ready to launch with `/let-it-cook`?"*
