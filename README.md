# ⚡ D-Level: Autonomous Software Delivery Engine

> A production-grade, composable AI agent skills collection based on the **[Frontier Engineering manifesto by Kiro](https://kiro.dev/topics/frontier-engineering)**.  
> The autonomous delivery engine for AI coding agents (Antigravity, Claude Code, Cursor, Windsurf, OpenCode).

[![Skills.sh](https://img.shields.io/badge/skills.sh-ThinhVu%2Fd--level-purple.svg)](https://skills.sh)
[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 📖 Background & Philosophy

Software development has split in two: those who simply changed their coding tools, and those who **changed how they work with AI agents**. Frontier developers hand-write less than 1% of the code they ship. Instead of typing syntax, they build the *agent setup that builds the software*.

This repository transforms the **10 Principles of Frontier Engineering** from [kiro.dev](https://kiro.dev/topics/frontier-engineering) into an executable, modular suite of AI skills.

Every skill is built upon two non-negotiable architectural tenets:
1. **Unix Philosophy: "Do one thing and do it best"**: Each skill has a single, razor-sharp responsibility. No bloated, confusing multi-tools.
2. **Autonomous & Composable**: 100% self-contained autonomous engine with zero external dependencies, designed to compose seamlessly into any existing agent setup or CI/CD workflow.

---

## 🚀 Quickstart

```bash
npx skills@latest add ThinhVu/d-level
```

Then initialize your repository:

```
/setup-d-level
```

This scans your workspace and configures:
- **Steering Contract (`AGENTS.md`)**: Detects fast test/lint loops, architectural boundaries, and safety guardrails.
- **Continuous Meta-Tuning (`.d-level/`)**: Establishes the failure-capture flywheel so your agent continually improves.

---

## 🏗️ Architecture & Division of Responsibilities

A common failure mode in AI-assisted development is lack of clear boundaries—agents jumping into unstructured coding without intent, verification, or guardrails.

The D-Level suite provides an end-to-end, structured engineering workflow covering every layer of the delivery lifecycle:

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                   THE D-LEVEL AGENTIC DELIVERY LIFECYCLE                    │
├──────────────────────────────────────┬──────────────────────────────────────┤
│  STRATEGIC & ARCHITECTURAL STAGES    │  EXECUTION & VERIFICATION ENGINE     │
├──────────────────────────────────────┼──────────────────────────────────────┤
│ • Requirements & Intent Capture      │ • Autonomous Long-Running Loops      │
│   (/write-intent: testable criteria) │   (/let-it-cook: unsupervised loop)  │
│ • Architecture RFC & Invariant Specs │ • Sub-10s Automated Feedback Harness │
│   (/draft-rfc: distributed trade-offs│   (/fast-loop: mocks, browser test)  │
│ • Parallel Architectural Spikes      │ • Enterprise Subsystem Modernization │
│   (/spike-off: evidence-based winner)│   (/agent-complete: 4-phase parity)  │
│ • Incident & System Postmortems      │ • Deep Staff Engineer Review         │
│   (/postmortem: blameless 5-Whys)    │   (/staff-review: blast radius/leaks)│
│ • Operational Playbooks & Runbooks   │ • Property-Based Correctness Testing │
│   (/runbook: copy-paste recovery)    │   (/property-test: fuzz randomized)  │
│ • Codebase Agent-Ready Scaffolding   │ • Automated Release & Verification   │
│   (/agent-ready: strict types/scripts│   (/let-it-ship: impact & protocol)  │
└──────────────────────────────────────┴──────────────────────────────────────┘
```

### 100% Self-Contained Autonomy
This suite provides an end-to-end, self-sufficient framework out of the box:
- **Project Steering Contract**: Set up steering and guardrails (`/setup-d-level`).
- **Autonomous Delivery**: Execute multi-ticket batches with circuit breakers (`/let-it-cook`).
- **Legacy Modernization**: Deep audit and migration with behavioral parity (`/agent-complete`).
- **Correctness & Safety**: Fuzz critical invariants (`/property-test`) and shield credentials (`/guardrails`).
- **Release Orchestration**: Automated changelogs, production impact assessment, and verification protocols (`/let-it-ship`).
- **Continuous Meta-Tuning**: Learn from agent friction and self-improve (`/tune-up`).

---

## 🔄 The D-Level Teams Lifecycle: From Intent to Production

D-Level engineering teams do not jump straight into typing syntax. As highlighted by [Kiro's Frontier Teams analysis](https://kiro.dev/topics/frontier-teams/), production teams restructure their workflow into 4 synchronized stages:

```
┌───────────────────┐      ┌───────────────────┐      ┌───────────────────┐      ┌───────────────────┐
│ 1. CAPTURE INTENT │      │ 2. DESIGN ARCH.   │      │ 3. BUILD CONTEXT  │      │ 4. VERIFY CORRECT │
│   /write-intent   │ ───> │   /draft-rfc      │ ───> │   /let-it-cook    │ ───> │   /property-test  │
│                   │      │   /spike-off      │      │   /safe-rewrite   │      │   /staff-review   │
└───────────────────┘      └───────────────────┘      └───────────────────┘      └───────────────────┘
          │                          │                          │                          │
          ▼                          ▼                          ▼                          ▼
   Acceptance Criteria,       System Topologies,         Autonomous Loop,          Adversarial Fuzzing,
   Boundary Invariants        Dependency Sequences       Notification Hooks        Property Verification,
   & Edge-Case Specs          & Spike Benchmarks         & Self-Correction         & Staff Diff Audit
```

---

## 📚 Complete Skill Catalog

Every skill adheres strictly to **"Do one thing and do it best"**:

### 🧭 Navigation & Setup

#### 1. [`/setup-d-level`](skills/setup-d-level/SKILL.md)
- **Core Tenet**: Building the setup that builds the software.
- **Single Responsibility**: Run-once repository initialization. Scans project tools, generates `AGENTS.md`, configures fast feedback commands, scaffolds `.d-level/scripts/notify.js`, and initializes `.d-level/`.

#### 2. [`/ask-d-level`](skills/ask-d-level/SKILL.md)
- **Core Tenet**: Workflow guidance & cognitive leverage.
- **Single Responsibility**: Your chief-of-staff navigator. Describe what you want to achieve, and it outlines the exact sequence of skills to invoke across the 4-phase lifecycle.

---

### 🔨 Core Feature Development Flow

#### 3. [`/write-intent`](skills/write-intent/SKILL.md)
- **Kiro Principle 1**: [You are the architect, not the typist](https://kiro.dev/topics/frontier-engineering/architect-not-typist/)
- **Single Responsibility**: Locks down the "Definition of Done", acceptance criteria, invariants, and edge cases before a single line of code is written.

#### 4. [`/let-it-cook`](skills/let-it-cook/SKILL.md)
- **Kiro Principle 2**: [Maximize agent time, minimize your involvement](https://kiro.dev/topics/frontier-engineering/maximize-agent-time/)
- **Single Responsibility**: Unloops the developer. Executes long-running tasks (30+ minutes) with autonomous self-correction loops, sub-task notification hooks, and circuit-breaker alerts.

#### 5. [`/fast-loop`](skills/fast-loop/SKILL.md)
- **Kiro Principle 4**: [Give agents a fast feedback loop](https://kiro.dev/topics/frontier-engineering/fast-feedback-loop/)
- **Single Responsibility**: Builds or tunes local testing harnesses (sub-10s unit tests, linters, headless browser checks, service mocks) so agents self-validate in tight loops.

#### 6. [`/property-test`](skills/property-test/SKILL.md)
- **Frontier Teams Tenet**: [Correctness by design, not just speed by default](https://kiro.dev/topics/frontier-teams/)
- **Single Responsibility**: Derives mathematical and behavioral invariants from acceptance criteria and generates randomized property-based tests (`fast-check`, `hypothesis`, `rapid`, `proptest`) probing 500+ edge cases before shipping.

#### 7. [`/staff-review`](skills/staff-review/SKILL.md)
- **Kiro Principle 7**: [Hold AI output to human standards](https://kiro.dev/topics/frontier-engineering/human-standards/)
- **Single Responsibility**: Deep 5-pass architectural review on `git diff` (logic correctness, boundary leakage, security vulnerabilities, performance regressions, test depth).

---

### 🏛️ Architecture & Refactoring

#### 8. [`/agent-ready`](skills/agent-ready/SKILL.md)
- **Kiro Principle 3**: [Build your codebase for agents](https://kiro.dev/topics/frontier-engineering/build-for-agents/)
- **Single Responsibility**: Audits and prepares codebases for agents, and performs **Brownfield Modernization** by extracting AS-IS specifications from legacy, undocumented systems.

#### 9. [`/spike-off`](skills/spike-off/SKILL.md)
- **Kiro Principle 5**: [Execution is cheap. Direction is everything.](https://kiro.dev/topics/frontier-engineering/direction-over-execution/)
- **Single Responsibility**: Settles architectural debates between competing options (e.g. Library A vs Library B) by building parallel spikes and delivering an evidence-based benchmark scorecard.

#### 10. [`/safe-rewrite`](skills/safe-rewrite/SKILL.md)
- **Kiro Principle 6**: [Treat code as disposable](https://kiro.dev/topics/frontier-engineering/code-is-disposable/)
- **Single Responsibility**: Locks external boundary invariant tests, discards old tangled implementations, and rebuilds cleanly from scratch with 100% black-box contract preservation (Public APIs, SDKs, Shared Libraries).

#### 11. [`/smart-rewrite`](skills/smart-rewrite/SKILL.md)
- **Deep Modules & Clean Architecture**: Unifies application subsystems, migrates consumers to a canonical pipeline, tears down transitional scaffolding, and eliminates dead code without cargo-cult backward compatibility wrappers.

#### 12. [`/agent-complete`](skills/agent-complete/SKILL.md)
- **Enterprise Modernization Engine**: The Sovereign Law of Legacy Modernization.
- **Single Responsibility**: End-to-end industrial migration orchestrator. Delivers the "Definition of Done" (DoD) to complement `/agent-ready` (DoR). Executes a 4-phase gated pipeline: Deep Reverse-Engineering Audit (5 logic pillars) ➔ Invariant Schemas & Tracer-Bullet Tickets ➔ Autonomous Parity Loop (Contract test first) ➔ Parity Verification & Staff Review.

#### 13. [`/guardrails`](skills/guardrails/SKILL.md)
- **Kiro Principle 8**: [Trust the boundaries, not the agent](https://kiro.dev/topics/frontier-engineering/trust-the-boundaries/)
- **Single Responsibility**: Enforces file sandboxes, gates irreversible commands (force pushes, deletions), and shields production credentials for safe unsupervised execution.

---

### 📑 Beyond Code: Operations & Architecture Docs

#### 14. [`/draft-rfc`](skills/draft-rfc/SKILL.md)
- **Kiro Principle 9.1**: [Use agents for everything — Architecture Design](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)
- **Single Responsibility**: Generates comprehensive Architecture RFCs (`docs/rfc/XXXX-<title>.md`) with context, Mermaid topology diagrams, tradeoffs, and zero-downtime rollout plans.

#### 15. [`/postmortem`](skills/postmortem/SKILL.md)
- **Kiro Principle 9.2**: [Use agents for everything — Incident RCA](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)
- **Single Responsibility**: Investigates production failures from logs/traces, traces git commits, executes a 5-Whys root cause analysis, and produces a blameless postmortem.

#### 16. [`/runbook`](skills/runbook/SKILL.md)
- **Kiro Principle 9.3**: [Use agents for everything — Operational Procedures](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)
- **Single Responsibility**: Writes executable operational runbooks (`docs/runbooks/<task>.md`) with pre-flight checks, exact shell commands, health verifications, and rollback steps.

#### 17. [`/let-it-ship`](skills/let-it-ship/SKILL.md)
- **Kiro Principle 9.4**: [Use agents for everything — Release Notes & Changelogs](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)
- **Single Responsibility**: Automated release publisher. Inspects git commit history, determines SemVer bumps, compiles Keep a Changelog entries (`CHANGELOG.md`), drafts executive GitHub Release Notes, compiles Production Impact Assessments (`docs/releases/vX.Y.Z-impact.md`), and triggers release notification webhooks.

---

### 🔄 Continuous Meta-Tuning

#### 18. [`/tune-up`](skills/tune-up/SKILL.md)
- **Kiro Principle 10**: [Continuously tune your agent setup](https://kiro.dev/topics/frontier-engineering/tune-your-setup/)
- **Single Responsibility**: The learning flywheel. When an agent stumbles, `/tune-up` analyzes the root cause and appends a permanent, enforceable rule into `AGENTS.md` and `.d-level/tuning-log.md`.

---

## 📂 Repository Structure

```
skills/
├── skills/                        # 18 Standard AI Agent Skills (SKILL.md format)
│   ├── setup-d-level/SKILL.md
│   ├── ask-d-level/SKILL.md
│   ├── write-intent/SKILL.md
│   ├── let-it-cook/SKILL.md
│   ├── fast-loop/SKILL.md
│   ├── property-test/SKILL.md
│   ├── staff-review/SKILL.md
│   ├── agent-ready/SKILL.md
│   ├── spike-off/SKILL.md
│   ├── safe-rewrite/SKILL.md
│   ├── smart-rewrite/SKILL.md
│   ├── agent-complete/SKILL.md
│   ├── guardrails/SKILL.md
│   ├── draft-rfc/SKILL.md
│   ├── postmortem/SKILL.md
│   ├── runbook/SKILL.md
│   ├── let-it-ship/SKILL.md
│   └── tune-up/SKILL.md
├── templates/                     # Standard D-Level templates
│   ├── AGENTS.md.template         # Core steering contract
│   ├── CHANGELOG.md.template      # Standard Keep a Changelog template
│   ├── release-impact.md.template # Production release impact assessment
│   ├── deep-spec.md.template      # Phase 1 deep reverse-engineering audit
│   ├── notify.js.template         # Background hook notification adapter
│   ├── notifications.json.template # Webhook credentials configuration
│   └── tuning-log.md.template     # Continuous meta-tuning record
├── package.json                   # Repository metadata
├── LICENSE                        # MIT License
└── README.md                      # Complete documentation
```

---

## 🌐 Universal Agent Compatibility

Tested and compatible across all major agentic IDEs, coding CLIs, and workflows:
- **Google Antigravity** (`.gemini/config/skills` or `.agents/skills`)
- **Anthropic Claude Code** (`~/.claude/skills` or `.claude/skills`)
- **Cursor** (`.cursorrules` & `.cursor/skills`)
- **Windsurf** (`.windsurfrules` & cascade skills)
- **AWS Kiro** (native spec-driven engine)
- **OpenCode & Codex**

---

## 📄 License & Credits

- Created and curated by **Thinh Vu ([@thinhvu](https://github.com/ThinhVu))**.
- Grounded in the engineering manifesto by **[Kiro.dev](https://kiro.dev/topics/frontier-engineering)** & **[Frontier Teams](https://kiro.dev/topics/frontier-teams/)**.
- Licensed under the **MIT License**.