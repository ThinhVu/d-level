---
name: tune-up
description: Reviews recent agent mistakes or frictions, identifies root causes, and crystallizes permanent rules into AGENTS.md and tuning-log.md to prevent repeat failures. Triggers on /tune-up.
---

# /tune-up

> **Principle 10**: Continuously tune your agent setup.  
> *"Every time an agent makes a wrong turn or pulls you into the loop unnecessarily, ask yourself: how do I prevent this from happening again?"*  
> **Source**: [kiro.dev/topics/frontier-engineering/tune-your-setup](https://kiro.dev/topics/frontier-engineering/tune-your-setup/)

## Purpose & Objective
This is the **meta-learning feedback loop** of Frontier Engineering.
Whenever an agent hallucinates an import, uses a deprecated API, breaks a coding style rule, or pulls you into the loop unnecessarily, you run `/tune-up`.
It analyzes the incident, discovers why the agent stumbled, and writes a crisp, permanent steering rule into `AGENTS.md` (or generates a helper script/skill) so **no agent ever makes that mistake again**.

## When to Reach for It
- Immediately after correcting an agent's wrong turn or hallucination.
- When an agent needed manual intervention that could have been automated with a script or rule.
- At the end of a sprint or milestone to consolidate agent learnings.
- Triggered by typing `/tune-up [description of mistake or friction]`.

## Standalone & Synergy Characteristics
- **Standalone**: Directly maintains `.frontier/tuning-log.md` and appends verified guidelines to `AGENTS.md`.
- **Synergy with Frontier Suite**: Powers the continuous learning flywheel across all skills—capturing mistakes from `/let-it-cook` or postmortems from `/postmortem` to update steering rules so agents never repeat the same mistake.

---

## Execution Workflow

### Step 1: Root Cause Diagnosis
Ask: *Why did the agent fail?*
- Was the convention implicit in the developer's head rather than written down?
- Was an old library version assumed because types were missing?
- Did a tool fail to output helpful error line numbers?

### Step 2: Formulate the Steering Rule
Convert the lesson into an unambiguous, agent-actionable directive.
- ❌ *Bad Rule*: "Write clean code." (Too vague)
- ✅ *Good Rule*: "When querying user permissions, always use `hasPermission(user, perm)` from `@auth/guards`; never check `user.roles` directly." (Specific, enforceable)

### Step 3: Commit the Rule & Update Log
1. Append the new rule to the appropriate section of `AGENTS.md`.
2. Record the retrospective entry in `.frontier/tuning-log.md`:
```markdown
### [YYYY-MM-DD] Prevent Direct User Role Checking
- **Context**: Agent implemented a permission check by inspecting `user.roles.includes('ADMIN')`.
- **Friction**: Bypassed RBAC hierarchy and caused permission leakage.
- **Root Cause**: Missing explicit guideline in AGENTS.md.
- **Steering Rule Added**: Section 4 of AGENTS.md updated with exact guard function import.
```
3. Report the update to the user: *"Steering file updated. Future agent sessions will permanently adhere to this rule."*
