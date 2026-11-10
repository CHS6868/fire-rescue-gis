# FireRescue GIS — Cost & Effort Estimation Plan

**Version:** Draft 0.2 · 2026-11-10  
**Owners:** Both project collaborators  
**Method:** Estimation by Analogy + Bottom-Up Estimation, reviewed through Expert Judgment

## 1. Estimation Objective

Estimate the **development effort** and potential **cash expenses** of a two-person, educational FireRescue GIS MVP. These numbers are initial planning assumptions, **not actual timesheets, fixed-price quotations, or guaranteed delivery dates**.

The baseline scope is defined in the [SRS](../requirements/SRS.md): a real basemap, **fictional** fire incidents and resources, manual incident creation, incident lifecycle, demonstration road routing, WebSocket notifications, authentication, persistence, and automated tests.

## 2. Estimation Techniques and Choice

The software engineering course material discusses algorithmic cost modeling, expert judgment, estimation by analogy, Parkinson's Law, and pricing-to-win.

| Method | How the team will use it | Suitability |
|---|---|---|
| **Estimation by Analogy** | Compare familiar CRUD, authentication, and API work with the previous cinema booking project. | Primary |
| **Bottom-Up Estimation** | Estimate each work package, then sum person-hours. | Primary |
| **Expert Judgment** | Ask the second developer or supervisor to challenge estimates. | Cross-check |
| **Algorithmic Models / COCOMO** | Possible later once credible size and historical parameters exist. | Optional |
| **Parkinson's Law** | Do **not** treat available time as proof the work takes exactly that long. | Not a reliable estimator |
| **Pricing-to-Win** | Not applicable to an unpaid student learning project as an effort-estimation method. | Not recommended |

## 3. Work Breakdown Structure (WBS) & Initial Effort

Unit = **person-hour** (1 person working for 1 hour). Simultaneous work by two people counts as 2 person-hours per hour elapsed.

| WBS | Work package | Low | High | Rationale |
|---|---|---:|---:|---|
| W1 | Requirements, design, diagrams, reviews | 12 | 18 | Prior SRS/ERD work reduces start-up effort |
| W2 | React interface, map integration, resource markers | 20 | 30 | React and Leaflet learning/integration |
| W3 | Express backend, database, authentication | 25 | 40 | Auth, schema/migrations, resource APIs |
| W4 | Incident lifecycle and fictional resource management | 20 | 30 | Forms, validation, history, scenario isolation |
| W5 | Routing adapter, demonstration route display | 15 | 25 | API limits, coordinates, error handling |
| W6 | Socket.IO events and reconnection behavior | 10 | 15 | Two-client state synchronization |
| W7 | Automated testing, deployment, documentation | 20 | 30 | E2E, API, CI and handover |
| | **Subtotal** | **122** | **188** | |
| | **20% planning contingency** | **24.4** | **37.6** | Risk allowance, not additional features |
| | **Total planning range** | **146.4** | **225.6** | Approx. **146–226 person-hours** |

**Boundary:** This estimate excludes an operational emergency system, actual live-sensor detection, advanced rescue modules, medical records, and a full hydrant inspection workflow. Full dashboard analytics and auto-generated scenarios are enhancement items and should be separately estimated before inclusion.

## 4. Capacity & Schedule Feasibility

Planning assumption: **2 developers × 8 hours each/week = 16 person-hours/week**.

| Scenario | Total effort | Nominal weeks at 16 person-hours/week |
|---|---:|---:|
| Low estimate including contingency | 146.4 | 9.2 weeks |
| High estimate including contingency | 225.6 | 14.1 weeks |
| Ten-week capacity | 160 available | 10 weeks |

**Implication:** A 10-week plan is plausible only toward the lower end of the effort range. If actual work tracks toward the high end, defer enhancement tasks or extend schedule; do not cut security, testability or the Simulation Only labeling.

The 10 phases in the README are **milestones**, not promises that each must take exactly one week. Some items overlap; actual work and dependencies will determine progress.

## 5. Proposed Effort Distribution (Initial, Not a Fixed Assignment)

| Area | Frontend lead | Backend lead | Shared review |
|---|---|---|---|
| Map components, UI forms | Lead | Assist | Both |
| Database, migrations, API contracts | Assist | Lead | Both |
| Routing and Socket.IO integration | UI client | Backend adapter | Both |
| Requirement changes & architecture | Shared | Shared | Both |
| Tests, acceptance, documentation | Shared | Shared | Both |

Split by **issue**, not by permanent skill silo. Both members should review each other's work and understand the deployed architecture.

## 6. Cash Cost (MYR) — Track Separately from Effort

No up-to-date service pricing is assumed here. Confirm the current pricing/quotas before choosing a provider.

| Category | Baseline approach | Budget handling |
|---|---|---|
| GitHub | Existing repository | Verify current plan/Actions quota |
| Frontend hosting | Free/starter tier if available | Record actual chosen plan and invoice |
| Backend hosting | Free/starter/student option if suitable | Check sleeping apps, bandwidth, billing |
| PostgreSQL hosting | Managed starter tier if suitable | Monitor project quota and backups |
| Routing API | Limited development API access | Monitor request counts and limits |
| Map tiles | Licensed/allowed provider | Attribution, fair usage, possible hosting fees |
| Domain name | Use platform-provided subdomain initially | Custom domain deferred |
| Labor | Student time | Report in person-hours; not RM0 'economic cost' |

**Suggested cash-budget table to fill after provider selection:**

| Item | Provider | Expected MYR/month | Actual MYR/month | Quota/notes | Approved by |
|---|---|---:|---:|---|---|
| Frontend hosting | TBD | TBD | TBD | | |
| Backend hosting | TBD | TBD | TBD | | |
| Database | TBD | TBD | TBD | | |
| Map / Routing | TBD | TBD | TBD | | |

## 7. Estimation Update Procedure

1. At the start of each milestone, break the work into GitHub Issues with acceptance criteria.
2. Estimate each issue in person-hours (optimistic / most-likely / pessimistic if helpful).
3. Record actual hours weekly; distinguish learning, coding, testing and rework.
4. Compare cumulative actuals against the remaining budget.
5. If remaining effort exceeds realistic capacity, raise a scope/schedule change issue.
6. Re-estimate after the first two coding milestones using observed work, not only analogy.

### Weekly Effort Log Template

| Week | Issue / deliverable | Owner | Planned h | Actual h | Remaining h | Notes |
|---|---|---|---:|---:|---:|---|
| W1 | Requirement review | Both | TBD | — | TBD | Pending |
| W2 | Project scaffold | Both | TBD | — | TBD | Pending |

## 8. Cost Estimation Risks

- Novelty of React Leaflet, PostGIS and Socket.IO can increase integration time.
- Third-party API limits/outages may generate rework.
- Uncontrolled additions from broader rescue and fire-prevention ideas can invalidate the estimate.
- Personal coursework schedules can reduce team capacity.

Cross-reference [Risk Register](risk-register.md). Review costs **weekly** and after any approved scope change.

## 9. Approval / Revision

- **Current baseline:** Draft only; confirm both contributors' weekly availability and the final Must-have list.
- **Approval:** Pending two-person review.
- **Re-estimation trigger:** Scope change, two consecutive missed milestones, or observed effort exceeding 20% of estimates.

Related: [README](../../README.md) · [SRS](../requirements/SRS.md) · [Communication Plan](communication-plan.md).
