# FireRescue GIS — Software Quality Assurance (SQA) Plan

**Version:** Draft 0.2 · 2026-11-10  
**Status:** Proposed verification criteria; not yet tested

## 1. Purpose and Quality Policy

Define how the two-developer team will prevent, discover, prioritize, fix, and verify software defects in the **educational FireRescue GIS simulation**.

**Safety boundary:** The app uses a real basemap with fictional incidents, stations, trucks and hydrants. It is **not an operational emergency system**. The UI and exported material must not imply actual fire detection, verified water availability, emergency-optimal routes or dispatch authority.

## 2. Quality Attributes and Priorities

The course material identifies Safety, Security, Reliability, Resilience, Robustness, Understandability, Testability, Adaptability, Modularity, Complexity, Portability, Usability, Reusability, Efficiency and Learnability.

The plan emphasizes six immediate priorities without ignoring the others:

| Quality attribute | Meaning in this project | Proposed evidence |
|---|---|---|
| **Safety / truthful representation** | Clearly label fictional events/resources and route limitations | UI checks + screenshot audit |
| **Security** | Protect sessions, enforce backend roles, keep API keys secret | Unauthorized API tests + review |
| **Reliability** | Persist committed records and maintain consistent state | Restart and transaction tests |
| **Resilience / Robustness** | Handle route service failures, network disconnects, invalid inputs | Negative/failure-path tests |
| **Usability / Learnability** | Make scenario selection, event creation and map exploration straightforward | Task-based UAT |
| **Testability / Modularity** | Isolate state transitions, coordinate utilities, routing adapter | Automated unit/integration tests |

**Additional attributes:** Efficiency is assessed using response-time targets; Portability through target-browser tests; Reusability through small domain modules; Adaptability through provider adapters; Understandability and Complexity through code review, naming, documentation and avoidance of overengineering.

## 3. Measurable Quality Requirements

These are **candidate acceptance targets**, not measured performance claims. The team must define test devices, network, dataset and sample size before treating them as release gates.

| ID | Attribute | Proposed target | Verification |
|---|---|---|---|
| QA-N01 | Performance | >=95% of ordinary database-backed API calls <=500 ms in test environment; exclude third-party API time | Timed API test |
| QA-N02 | Real time | >=95% of simulated event updates appear on another connected client within 2 s on stable test network | Two-client timestamp test |
| QA-N03 | Reliability | Committed incident/history survives app restart and page refresh | Integration / manual restart test |
| QA-N04 | Security | Viewer write operations always return 403; unauthenticated protected operations rejected | API negative tests |
| QA-N05 | Scenario isolation | No record from another scenario appears via API/socket channel | Multi-scenario tests |
| QA-N06 | Resilience | Routing errors, rate limits and timeouts show explicit unavailable state and no invented result | Mock provider tests |
| QA-N07 | Safety | Every incident/resource/route-facing screen clearly communicates simulated nature and route limitations | UI checklist / E2E |
| QA-N08 | Usability | In a planned UAT, test users can complete the basic scenario task with minimal guidance | Structured observation |
| QA-N09 | Maintainability | Core business functions tested independently; PR review completed | CI + peer review |
| QA-N10 | Compatibility | Core flow works on target current desktop browsers, and main layout functions at mobile width | Cross-browser / responsive tests |

## 4. Quality Assurance vs Quality Control

- **QA (process-oriented):** Definition of Done, review rules, coding conventions, requirements traceability, CI, documentation updates.
- **QC (product-oriented):** Running tests, inspecting screens, validating calculations and detecting defects.

Every feature issue should reference at least one SRS requirement and explicit acceptance criteria.

## 5. Test Strategy

| Level | Scope | Proposed tools | Sample tests |
|---|---|---|---|
| Unit | Pure functions, validation, coordinates, state machine | Vitest | Allowed transitions, lat/lon conversion |
| Component | React form, popups, state and error messages | Vitest + React Testing Library (if selected) | Missing field, loading/failure UI |
| Integration | Express + database, auth, scenario isolation, routing adapter | API test harness, mocked provider | Atomic incident history, permissions |
| End-to-end | Full browser workflows | Playwright | Login → scenario → create incident → view map |
| Real-time integration | Two browser clients, reconnect | Socket.IO test harness/Playwright | Broadcast, reconnection sync |
| Usability / UAT | Task completion and clarity | Short test script | Can user identify Simulation Only? |
| Non-functional | Performance, browser support, failure recovery | API timing, browser matrix | Under load, timeout, responsive UI |

Run routing tests against a **mock service** in CI to avoid third-party quota dependency. Include a separate, controlled smoke test against the selected provider before release.

## 6. Prioritized Test Cases

| Test ID | Requirement | Input/Action | Expected Result | Priority |
|---|---|---|---|---|
| TC-01 | FR-01 | Valid login then logout | Protected session established then removed | P0 |
| TC-02 | FR-03 | Viewer sends PATCH to incident status endpoint | 403; no data modification | P0 |
| TC-03 | FR-05–08 | Load dashboard with fictional seed markers | Map and markers visible with attribution and simulation notice | P0 |
| TC-04 | FR-10–11 | Create valid simulated incident | Record persists; marker appears | P0 |
| TC-05 | FR-10 | Submit missing severity / invalid coordinates | 4xx validation error; no record | P0 |
| TC-06 | FR-12–13 | Valid transition `REPORTED` to `ACKNOWLEDGED` | Incident and history update together | P0 |
| TC-07 | FR-12–13 | Invalid state transition | Rejected; prior status/history unchanged | P0 |
| TC-08 | FR-19, FR-37 | Attempt cross-scenario resource retrieval/edit | Unauthorized/inconsistent request rejected | P0 |
| TC-09 | FR-22–25 | Mock service returns route GeoJSON | Map renders returned line and numeric metrics | P0 |
| TC-10 | FR-27 | Mock timeout / 429 / no route | Explicit failure message; no fabricated result | P0 |
| TC-11 | FR-28–29 | Create event in browser A; watch browser B | Correct scenario receives update | P0 |
| TC-12 | FR-31 | Disconnect, modify state, reconnect | Browser re-fetches current server state | P0 |
| TC-13 | NFR-11 | Open all incident/resource/route screens | Clear simulation-only language on each | P0 |
| TC-14 | FR-21 | Show hydrant with null inspection date | Displays 'Not recorded (simulated)' or equivalent | P1 |
| TC-15 | FR-32–34 | Compare statistic counts to fixture data | Dashboard equals database results | P1 |
| TC-16 | NFR-14 | Rebuild demo database using migrations + seeds | Expected schema and fictional fixtures recreated | P0 |

**P0** = release-blocking for the corresponding MVP feature; **P1** = enhancement or usability refinement.

## 7. CI / GitHub Quality Gates

Every PR targeting `main` should run at least:

1. Install dependencies from lockfiles.
2. TypeScript type checking (frontend/backend, if configured).
3. Linting / formatting validation.
4. Unit tests.
5. API/integration tests against disposable test data, as available.
6. Build frontend and backend.

Browser E2E suites may run on PR or on scheduled/main branch CI depending on time and quota. Never run CI against production user data or active secrets. Use GitHub Secrets and dummy/mock credentials where appropriate.

**Merge rule:** Another developer reviews the PR; P0 tests pass and no critical security/safety issue remains unresolved.

## 8. Defect Severity and Workflow

| Severity | Meaning | Release policy |
|---|---|---|
| **Critical (S1)** | Security breach, real-world misrepresentation risk, serious data corruption | Blocks public demo until fixed |
| **High (S2)** | Core incident creation, auth, persistence or routing error handling broken | Blocks MVP release |
| **Medium (S3)** | Secondary function defect; workaround exists | Fix before release if feasible, otherwise document |
| **Low (S4)** | Cosmetic or minor text defect | Backlog, prioritize as needed |

Workflow: **New Issue → Reproduce → Assign severity → Fix in branch → Test / Review → Close**. Include steps to reproduce, expected versus actual behavior, logs/screenshots without secrets, and linked test.

## 9. Quality Metrics and Reports

Track weekly:

- P0/P1 test pass counts and failure details
- Number of open Critical and High defects
- PR review completion rate
- Requirements covered by at least one planned test
- Routing error handling success during tests
- Measured API latency and event update delay under named conditions
- Known UI usability issues and simulation-label audit results

Avoid reporting inflated code coverage percentages as evidence of safety. Focus on **requirement-linked tests and critical failure paths**.

## 10. Definition of Done (Feature)

- [ ] Linked Issue and relevant FR/NFR identified
- [ ] Behavior meets acceptance criteria
- [ ] Backend validation and authorization applied when relevant
- [ ] Unit/integration/E2E coverage added or a documented test rationale provided
- [ ] Error, empty and loading states considered
- [ ] Simulation Only labeling and no real emergency data are preserved
- [ ] No secrets committed; provider terms observed
- [ ] Documentation and diagrams updated if contract/schema changed
- [ ] PR reviewed by collaborator; CI passes

## 11. MVP Release Acceptance Checklist

- [ ] AC-01–AC-13 from SRS reviewed against evidence
- [ ] No open S1/S2 defects for MVP functionality
- [ ] Demonstration data clearly fictional and scenario-isolated
- [ ] Route result displays caveat and fail-closed unavailable state
- [ ] Browser compatibility smoke tests complete
- [ ] Database migrations and seeds reproducible
- [ ] README, environment setup and limitations accurate
- [ ] Both collaborators approve release and tag version

## 12. Documentation and Test Artifacts

Store test cases, evidence, defect issues and test summary under `docs/testing/` or linked GitHub Issues. Maintain traceability from [SRS](../requirements/SRS.md) requirements to tests and bug fixes.

**Approval:** Pending team review and test-environment specification.  
Related: [README](../../README.md) · [Risk Register](../project-management/risk-register.md) · [Communication Plan](../project-management/communication-plan.md).
