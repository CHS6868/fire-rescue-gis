# FireRescue GIS — Project Communication & Collaboration Plan

**Version:** Draft 0.2 · 2026-11-10  
**Team size:** 2 developers  
**Primary collaboration platform:** GitHub

## 1. Purpose

Establish lightweight, reliable communication rules for a two-person software engineering project. Decisions that affect requirements, schema, interface contracts, security, estimates or delivery must be traceable even when discussed face to face.

## 2. Four Communication Management Processes

| Process | Meaning | Planned team practice | Evidence |
|---|---|---|---|
| **CP — Communications Planning** | Decide what, how, when and to whom to communicate. | Agree on schedule, responsibilities, channels and response expectations. | This plan, GitHub Milestones |
| **ID — Information Distribution** | Share approved information promptly. | Store requirements, decisions, API contracts and code in the repository. | Docs, Issues, PRs |
| **PR — Performance Reporting** | Report progress, risks and deviations. | Weekly 15–30 minute review and updated GitHub board. | Milestone notes, issue status |
| **AC — Administrative Closure** | Confirm acceptance and archive records. | Test and review deliverables, tag release, close milestone. | Release notes, test results, archived docs |

## 3. Roles and Responsibility Matrix (Draft)

| Deliverable | Frontend lead | Backend lead | Approval |
|---|---|---|---|
| SRS, scope and UX requirements | Co-author | Co-author | Both |
| React layout, Leaflet, forms | Lead | Reviewer | Both at PR |
| Express, database and APIs | Reviewer | Lead | Both at PR |
| Routing adapter and UI | Client/UI lead | Adapter/backend lead | Both |
| Socket.IO notifications and sync | Client implementation | Server implementation | Both |
| Unit, API and E2E testing | Shared | Shared | Both |
| CI, deployment and secrets | Reviewer | Initial lead | Both |
| Cost / risk register updates | Shared | Shared | Both |
| Documentation and release notes | Shared | Shared | Both |

The labels `Frontend lead` and `Backend lead` are initial responsibilities, **not permanent ownership boundaries**. Both people should review and understand the architecture.

## 4. Communication Matrix

| Topic | Channel | Cadence | Owner | Expected output |
|---|---|---|---|---|
| Weekly planning | Short in-person or online meeting | Once per week | Both | Updated Issue priorities and assignments |
| Daily blockers | Chosen chat channel | As needed | Person blocked | Blocker and proposed next step |
| Requirements changes | GitHub Issue + SRS PR | Whenever proposed | Proposer | Approval/rejection and impact record |
| Code changes | Feature branch + GitHub PR | Per deliverable | Developer | Reviewed, tested merge |
| API/schema changes | GitHub Issue/PR and docs | Before merging changes | Change owner | Updated contract/migration |
| High-risk issues | Issue tagged `risk` + direct message | Immediately | Discoverer | Mitigation and decision |
| Milestone report | GitHub Milestone/Issue summary | End of milestone | Both | Results, tests, deviations, next steps |
| Release closure | Release notes and tagged version | Per release | Both | Approved demo snapshot |

**Suggested response expectations:** Notify collaborator promptly when blocked; review ordinary PRs within ~2 days **when feasible**, without treating this as a hard service-level contract during coursework/exams.

## 5. Weekly Meeting Agenda (15–30 Minutes)

1. Show completed deliverables and merged PRs.
2. Check outstanding Issues against milestone goals.
3. Review actual hours versus estimated remaining hours.
4. Review the Risk Register (especially High risks).
5. Confirm interface changes and upcoming integration work.
6. Assign next tasks and record decisions.

### Meeting Notes Template

```md
## Weekly Review — YYYY-MM-DD
- Attendees:
- Completed (Issues / PRs):
- Current blockers:
- Actual hours this week:
- Open risks and changes:
- Decisions (with links):
- Next tasks and owners:
- Next review date:
```

Place meeting notes in a tracked Issue or `docs/project-management/meeting-notes/`, if useful; avoid committing personal chat transcripts.

## 6. GitHub Branch and Pull Request Rules

### 6.1 Branches

- `main`: stable, reviewed branch.
- `feature/<short-description>`: new functionality (e.g. `feature/incident-map-form`).
- `fix/<short-description>`: bug fixes.
- `docs/<short-description>`: documentation changes.
- `test/<short-description>`: test coverage.

After collaboration begins, **do not push unreviewed code directly to `main`**. Each collaborator works from an up-to-date base branch.

### 6.2 Issue Template (Suggested)

```md
## Goal
What user-visible or technical outcome should this achieve?

## Scope
What is included and excluded?

## Acceptance Criteria
- [ ] Observable criterion 1
- [ ] Observable criterion 2

## Dependencies / Risks
Related Issues / FR / NFR:

## Owner and Estimate
Owner:
Estimated person-hours:
```

### 6.3 Pull Request Checklist

```md
## Summary
- What changed, and why?

## Evidence
- [ ] Tests added or updated
- [ ] Relevant automated tests pass
- [ ] Screenshot/demo for UI change (if applicable)
- [ ] Requirement / API / ERD docs updated (if applicable)
- [ ] No credentials, real emergency data, or personal medical data committed
- [ ] Simulation-only labels remain present

## Review
- Reviewer:
- Related Issue:
```

Recommend **squash merging** routine feature PRs if both developers agree. Use consistent commit messages such as `feat: add simulated incident form`, `fix: handle routing timeout`, or `docs: update risk register`.

## 7. Requirement and Architecture Decisions

A technical decision should include:

- Decision (e.g. select provider, use socket rooms, use PostGIS now/later).
- Alternatives considered and trade-offs.
- Impact on existing diagrams, schema, code, tests, estimates, risks.
- Decision date and two-person approval.

Use small decision records such as `docs/decisions/ADR-001-routing-provider.md` for significant choices. This is optional but useful for portfolio evidence.

## 8. Disagreement and Conflict Resolution

1. State the technical disagreement as alternatives, not criticism of the person.
2. Compare options against the SRS, available time, API terms and acceptance criteria.
3. Run a small time-boxed spike or prototype if evidence is missing.
4. Choose by mutual agreement; record why in an Issue/ADR.
5. If there is no agreement, defer nonessential functionality rather than silently changing scope.

## 9. Reporting and Administrative Closure

At each milestone, post a concise **milestone report** containing:

- Planned versus completed Issues and demo screenshots
- Test results and known defects
- Actual versus estimated person-hours
- Open risks and mitigation owners
- Any SRS/ERD/API changes
- Next milestone entry criteria

Before release, both contributors check: key acceptance criteria passed, required disclaimer present, no exposed secrets, setup docs accurate, all critical defects closed, version tagged, and docs updated.

## 10. Communication Plan Review

Review this plan at the end of **Phase 1**, after the first cross-module integration, and whenever collaboration friction becomes significant.

**Approval:** Pending both developers' agreement on lead roles, weekly meeting day, and review expectations.

Related: [Cost Estimation](cost-estimation.md) · [Risk Register](risk-register.md) · [Quality Assurance Plan](../quality/quality-assurance-plan.md).
