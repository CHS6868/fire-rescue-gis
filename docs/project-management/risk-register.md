# FireRescue GIS — Risk Management Plan & Risk Register

**Version:** Draft 0.2 · 2026-11-10  
**Status:** Initial assessment; owners and ratings to be confirmed by both developers

## 1. Risk Management Process

The process follows four recurring stages:

1. **Risk identification:** identify project, product and relevant operational/misinterpretation risks.
2. **Risk analysis:** assess likelihood and impact.
3. **Risk planning:** define prevention, contingency, trigger and responsibility.
4. **Risk monitoring:** check risks weekly and when an incident or change occurs.

## 2. Scoring Rules

**Probability (P), scale 1–5:** 1 = rare, 2 = unlikely, 3 = possible, 4 = likely, 5 = very likely.  
**Impact (I), scale 1–5:** 1 = negligible, 2 = minor, 3 = moderate, 4 = major, 5 = critical.  
**Score = P × I.**

| Score | Initial response |
|---|---|
| **15–25 High** | Assign clear mitigation and review every week / after trigger |
| **8–14 Medium** | Track weekly, mitigate before affected milestone |
| **1–7 Low** | Monitor and reassess when conditions change |

Scores are subjective planning judgments, not measured probabilities.

## 3. Initial Risk Register

| ID | Risk | P | I | Score | Level | Owner | Early warning / trigger |
|---|---|---:|---:|---:|---|---|---|
| R01 | **Scope creep** from full firefighting/rescue/medical/prevention module list | 4 | 5 | 20 | High | Both | New feature requested without effort trade-off |
| R02 | Routing provider unavailable, blocked or rate-limited | 4 | 4 | 16 | High | Backend lead | 429s, outage, quota nearing limit |
| R03 | Demo data mistaken for real emergency intelligence | 3 | 5 | 15 | High | Both | Missing simulation label; screenshots circulated without disclaimer |
| R04 | Invalid lat/lon order or cross-scenario data mix-up | 3 | 4 | 12 | Medium | Both | Wrong marker placement or foreign data appears |
| R05 | Git conflicts or unreviewed changes break `main` | 3 | 3 | 9 | Medium | Both | Repeated conflicts / CI failures |
| R06 | Less study time than planned | 4 | 3 | 12 | Medium | Both | Two consecutive overdue milestones |
| R07 | Improper authentication or role enforcement | 3 | 5 | 15 | High | Backend lead | Viewer can call write endpoint |
| R08 | WebSocket disconnect creates stale client state | 3 | 3 | 9 | Medium | Backend lead | Data differs after reconnection |
| R09 | Database and incident-history inconsistency | 3 | 4 | 12 | Medium | Backend lead | Missing history or invalid FK |
| R10 | Secrets leaked into GitHub or browser bundle | 3 | 4 | 12 | Medium | Both | Key found in commit or frontend code |
| R11 | Map/routing license, attribution or usage noncompliance | 3 | 3 | 9 | Medium | Both | Attribution missing / terms not reviewed |
| R12 | Routing request latency impairs UI | 3 | 3 | 9 | Medium | Frontend lead | Spinner never ends / frequent timeouts |

## 4. Response Plans

| ID | Preventive mitigation | Contingency if triggered |
|---|---|---|
| R01 | Freeze V1.0 Must-haves; record additional ideas in V1.1 backlog; require both approvals. | Remove or defer Should/Could features; revise estimate and roadmap. |
| R02 | Server-side routing adapter, quotas, caching and mocked tests. | Show 'Route unavailable'; disable relevant action and record error; never fabricate route. |
| R03 | Prominent `SIMULATION ONLY — NOT FOR EMERGENCY USE` header, fictional seed records, no official branding. | Disable public demo until label/data issue is fixed. |
| R04 | Unified coordinate conversion helper; scenario-bound database/API validation and tests. | Roll back faulty change, repair test data, re-run cross-scenario tests. |
| R05 | Feature branches, small PRs, required peer review, passing CI. | Revert broken merge, resolve conflict together and re-run CI. |
| R06 | Weekly capacity check, two-week planning window, early flag of deadlines. | Rebalance issues, move enhancement tasks, revise planned finish. |
| R07 | Backend role middleware, negative API tests, secure sessions. | Block access, patch permissions, invalidate affected sessions as needed. |
| R08 | Reconnection handler re-fetches authoritative server state. | Display stale/offline indicator and force state refresh. |
| R09 | SQL constraints, migrations, transactions for status and history writes. | Restore from controlled seed/backup in demo environment; fix migration. |
| R10 | `.env` in `.gitignore`, secret scanning, server-side API key storage. | Rotate exposed keys immediately, revoke credentials, clean Git history if warranted. |
| R11 | Review provider terms and attribution, limit requests and avoid bulk tile downloads. | Stop noncompliant usage; replace provider or adjust deployment. |
| R12 | Route timeout, progress state and clear retry handling. | Show unavailable state; allow other functions to continue. |

## 5. Monitoring & Escalation

**Weekly review (10–15 minutes):**

- Has a new risk appeared?
- Did probability/impact change?
- Did any trigger occur? Record evidence (issue, screenshot, test log).
- Is the owner actively implementing the planned mitigation?
- Must the scope or schedule change?

**Escalate immediately** to both contributors for any exposed key, unauthorized write, wrong cross-scenario data, misleading real-emergency presentation, or an unbounded failure on the public demo.

### Risk Review Log

| Date | Risk ID | Previous score | New score | Observation / evidence | Action | Owner | Due |
|---|---|---:|---:|---|---|---|---|
| 2026-11-10 | R01–R12 | — | Initial | Draft analysis | Review with collaborator | Both | TBD |

### Status Definitions

`Open` → `Mitigating` → `Monitoring` → `Closed`. Record status per issue in GitHub; a closed risk can be reopened if conditions change.

## 6. Risk Ownership & GitHub Integration

- Create a GitHub Issue for each High risk: label `risk`, add severity, owner and milestone.
- Reference related FR/NFR/test cases from [SRS](../requirements/SRS.md) and [Quality Plan](../quality/quality-assurance-plan.md).
- Include risk review in each milestone's completion checklist.
- Document accepted residual risks in release notes, especially third-party routing limitations and simulation-only scope.

**Approval:** Pending review by both developers.  
Related: [Cost Plan](cost-estimation.md) · [Communication Plan](communication-plan.md) · [README](../../README.md).
