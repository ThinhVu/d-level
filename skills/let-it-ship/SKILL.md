---
name: let-it-ship
description: Automates production releases by inspecting git commit history, determining SemVer bump, generating Keep a Changelog entries, drafting GitHub Release Notes, and compiling Production Impact Assessments. Triggers on /let-it-ship.
---

# /let-it-ship

> **Principle 9.4**: Use agents for everything — Release Notes & Changelogs.  
> *"Non-code tasks have the same properties that make coding amenable to AI: clear inputs, structured outputs, and high leverage. You shouldn't spend 45 minutes manually parsing git logs to write a changelog."*  
> **Source**: [kiro.dev/topics/frontier-engineering/agents-for-everything](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)

## Purpose & Objective
After an autonomous build or refactor loop finishes (`/let-it-cook` or `/agent-complete`), manually auditing dozens of commits to draft release notes is slow and prone to omitting breaking changes.

`/let-it-ship` is the **automated release publisher**. It inspects git commit history between releases, filters out internal noise, determines the appropriate SemVer bump, updates `CHANGELOG.md` according to [Keep a Changelog](https://keepachangelog.com/), drafts executive release notes for GitHub Releases, and dispatches release announcements.

---

## When to Reach for It
- When preparing a new version release, sprint release, or milestone tag.
- When creating a customer-facing summary of recent updates.
- Triggered by typing:
  ```bash
  /let-it-ship [target version or commit range]
  ```

### Examples:
```bash
# Automatically detects commits since the last git tag to HEAD
/let-it-ship

# Explicitly specify target version
/let-it-ship v1.3.0

# Specify custom commit range
/let-it-ship v1.2.0..HEAD
```

---

## Standalone & Synergy Characteristics
- **Standalone**: Reads `git log`, inspects diffs, updates `CHANGELOG.md`, and writes `docs/releases/vX.Y.Z.md`.
- **Synergy with D-Level Suite**:
  - The natural culmination of `/let-it-cook` and `/agent-complete`.
  - Dispatches release announcements through `.d-level/scripts/notify.js` to Discord, Telegram, or Slack.

---

## Execution Workflow

### Step 1: Detect Release Boundary
The agent inspects repository git history:
1. Locate the latest release tag using `git describe --tags --abbrev=0 2>/dev/null` (or commit `HEAD~20` if no tags exist).
2. Collect commits in the target range: `git log <last-tag>..HEAD --oneline --no-merges`.

### Step 2: Filter Noise & Categorize
The agent filters out low-signal chore commits (`lint`, `typo`, `merge branch`, `bump deps`) and clusters meaningful changes into 5 standard categories:
- 🚀 **Added**: New features, public APIs, endpoints, or UI capabilities.
- ⚡ **Changed**: Performance optimizations, architecture refactoring, dependency upgrades.
- 🐛 **Fixed**: Bug fixes, edge-case handling, crash resolutions.
- ⚠️ **Breaking Changes**: Any altered API signature, changed schema shape, removed endpoint, or altered environment configuration.
- 🔒 **Security**: Vulnerability patches, secret shielding, permission tightening.

### Step 3: Determine SemVer Bump
Based on the categorized changes:
- **Major (`X.0.0`)**: If any breaking change is detected.
- **Minor (`0.X.0`)**: If new features were added without breaking changes.
- **Patch (`0.0.X`)**: If only bug fixes, chores, and minor optimizations were made.

### Step 4: Write Changelog, Release Notes, Impact Assessment & Verification Protocol
The agent produces a complete release documentation package:
1. **Update `CHANGELOG.md`**: Prepend the new version section at the top following [Keep a Changelog](https://keepachangelog.com/) standards.
2. **Draft GitHub Release Notes (`docs/releases/vX.Y.Z.md`)**: Executive summary highlight, categorized change bullets, and upgrade instructions.
3. **Generate Production Impact Assessment (`docs/releases/vX.Y.Z-impact.md`)**:
   - Uses `templates/release-impact.md.template`.
   - Analyzes **Blast Radius** across modified modules and downstream consumers.
   - Evaluates **Database & Migration Impact** (schema changes, lock risks, rollback feasibility).
   - Defines **Rollout Strategy, Smoke Checks & Rollback Triggers** for SRE/operations.
4. **Generate Autonomous Verification Protocol & Evidence (`docs/verification/<version>.md`)**:
   - Uses `templates/verification-protocol.md.template`.
   - Formulates machine-actionable test scenarios (routes, selectors, payloads, expected status/schema).
   - Evaluated autonomously via agent runners (`browser_subagent`, E2E test runs, API probes) to attach concrete **Evidence Artifacts** (screenshots, log traces, network payloads), enabling one-click human governance sign-off without manual clicking bottlenecks.

### Step 5: Safety Review & Dispatch Release Notification
1. **Safety Gate**: Never push release branches or tags to remote git (`git push`) without explicit user review and approval.
2. If `.d-level/scripts/notify.js` exists, dispatch the release announcement:
```bash
node .d-level/scripts/notify.js --event=batch_done --title="Release vX.Y.Z Ready to Ship" --message="Changelog, release notes, impact assessment & verification checklist compiled"
```
