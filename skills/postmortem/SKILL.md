---
name: postmortem
description: Performs blameless incident root cause analysis (5 Whys), traces failure timelines from error logs/traces, and produces production postmortem documents with concrete prevention items. Triggers on /postmortem.
---

# /postmortem

> **Principle 9.2**: Agents for everything — Incident RCA & Postmortems.  
> *"Operational investigations that took hours now take minutes."*  
> **Source**: [kiro.dev/topics/frontier-engineering/agents-for-everything](https://kiro.dev/topics/frontier-engineering/agents-for-everything/)

## Purpose & Objective
"Do one thing and do it best": `/postmortem` automates the operational post-incident investigation.
Feed it error logs, stack traces, commit IDs, or bug descriptions. It correlates timestamps, traces git commits, identifies the underlying root cause using the **5 Whys methodology**, and drafts a blameless postmortem (`docs/postmortems/YYYY-MM-DD-<title>.md`) with regression test requirements.

## When to Reach for It
- Immediately after recovering from a production outage, regression, or Sev-1/Sev-2 incident.
- When investigating recurring flaky bugs or critical customer-reported defects.
- Triggered by typing `/postmortem [paste logs, trace, or incident summary]`.

## Standalone & Synergy Characteristics
- **Standalone**: Generates complete blameless postmortems with timeline tables, root-cause graphs, and actionable preventive tickets.
- **Synergy with Frontier Suite**: Directly feeds preventive invariants into `/tune-up` (updating `AGENTS.md`) and operational fixes into `/runbook`, preventing repeat incidents.

---

## Output Postmortem Structure

Generated at `docs/postmortems/YYYY-MM-DD-[incident-slug].md`:
```markdown
# Incident Postmortem: [Incident Summary Title]

- **Date**: YYYY-MM-DD
- **Severity**: SEV-1 | SEV-2 | SEV-3
- **Duration / Impact**: 42 minutes | 8.3% of checkout requests failed

## 1. Executive Summary
What happened, what was the customer impact, and how was it mitigated?

## 2. Chronological Timeline (UTC)
| Time | Event Description |
|---|---|
| 14:02 | Deployment v2.4.1 completed. |
| 14:05 | P99 latency spike detected on `/orders`. |
| 14:12 | Incident acknowledged by on-call engineer. |
| 14:25 | Root cause identified: unindexed database column query lock. |
| 14:44 | Rollback to v2.4.0 completed; metrics normalized. |

## 3. Root Cause Analysis (The 5 Whys)
1. **Why did checkout fail?** Database connection pool was exhausted.
2. **Why was it exhausted?** Slow queries on `order_history` took 12 seconds each.
3. **Why did queries take 12s?** Full table scan on `created_at` column.
4. **Why was there a full scan?** Index was accidentally dropped in migration `0042`.
5. **Why was the index dropped without notice?** Migration test suite lacked an EXPLAIN query check.

## 4. Preventive Action Items
- [ ] Add regression boundary test verifying query plan index usage.
- [ ] Implement database pool circuit breaker.
- [ ] Update `/runbook` deployment checklist.
```
