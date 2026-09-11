---
name: staff-review
description: Performs rigorous, multi-pass code review like a Staff/Principal Engineer, checking for correctness, boundary leakage, security vulnerabilities, performance regressions, and architectural integrity. Triggers on /staff-review.
---

# /staff-review

> **Principle 7**: Hold AI output to human standards.  
> *"Speed without quality is just more risk in production. You own the code that ships under your name, regardless of who or what generated it."*  
> **Source**: [kiro.dev/topics/frontier-engineering/human-standards](https://kiro.dev/topics/frontier-engineering/human-standards/)

## Purpose & Objective
Reading every line of agent-generated code manually does not scale. But letting unvetted AI code slip into production is dangerous.
`/staff-review` acts as an automated **Staff / Principal Engineer reviewer**. It inspects your git diff with surgical rigor across 5 non-negotiable dimensions before any pull request is opened.

## When to Reach for It
- Before submitting a pull request or merging a feature branch into main.
- After an autonomous loop (`/let-it-cook`) has finished modifying multiple files.
- Triggered by typing `/staff-review [branch or commit range]`.

## Standalone & Synergy Characteristics
- **Standalone**: Analyzes `git diff` directly against production-grade engineering standards.
- **Synergy with Frontier Suite**: Acts as the senior quality gatekeeper following `/let-it-cook` or `/agent-complete`, auditing upstream/downstream blast radius, concurrency race conditions, secret leaks, and security boundary integrity before shipping via `/let-it-ship`.

---

## The 5-Pass Review Checklist

1. **Pass 1: Correctness & Logic Flaws**
   - Are edge cases handled (empty lists, null pointers, network failures, timeouts)?
   - Are off-by-one errors or unintended type coercions present?
2. **Pass 2: Boundary & Architecture Integrity**
   - Did the changes violate modular boundaries or create new circular dependencies?
   - Was duplicate logic introduced instead of reusing existing shared utilities?
3. **Pass 3: Security & Vulnerability Scanning**
   - SQL injection, unsanitized user input, XSS, SSRF risks.
   - Any hardcoded credentials, tokens, or exposed internal endpoints.
4. **Pass 4: Performance & Resource Leaks**
   - Unbounded queries (missing `LIMIT`/pagination).
   - N+1 query patterns or unclosed stream/database connections.
5. **Pass 5: Test Quality & Invariant Coverage**
   - Do tests assert meaningful behavior, or are they shallow tests that only check status 200 without payload assertions?

---

## Output Format
```markdown
# 🛡️ Staff Engineer Review Summary

### Verdict: CHANGES REQUESTED / APPROVED WITH COMMENTS

#### 🔴 Critical Blockers (Must fix before merge)
- [File: `src/api/orders.ts:42`] Missing transaction wrapper around inventory decrement and payment charge; risks partial state on network failure.

#### 🟡 Architectural Warnings (Recommended improvements)
- [File: `src/utils/format.ts:15`] Duplicate date formatter introduced; use `shared/date-utils.ts` instead.

#### 🟢 Positive Notes
- Clean test coverage on boundary edge cases.
```
