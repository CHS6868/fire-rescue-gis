# FireRescue GIS — Software Requirements Specification (SRS)

| Field | Value |
|---|---|
| Project | FireRescue GIS — Software Engineering Project 2 |
| Document | Software Requirements Specification |
| Version | Draft 0.2 |
| Date | 2026-11-10 |
| Status | **For collaborator review; requirements not yet baselined** |
| Intended system | Educational GIS simulation web application |

> **Simulation-only statement:** Real geographic basemap, **fictional** incidents and emergency resources. Never use this application for real fire detection, deployment, dispatch, hydrant verification, or emergency response.

## 1. Introduction

### 1.1 Purpose

Specify what the first version of FireRescue GIS must do, how well it should operate in a test environment, the relevant actors, the data it requires, and the conditions under which the two-person team can accept an MVP release.

### 1.2 Background and Problem Statement

For an **educational simulation**, managing incidents, fictional resources, and map routes through disconnected records makes it difficult to demonstrate how a GIS-enabled workflow operates. The project proposes a single interface for events, scenario data, mapping, and synchronized updates. This is a **proposed learning problem**, not a claim that real fire departments lack adequate systems.

### 1.3 Objectives

- **O1:** Interactive real-world basemap with clearly fictional markers.
- **O2:** Persistent simulated incident lifecycle and history.
- **O3:** Fictional stations, vehicles, and hydrants grouped by scenario.
- **O4:** On-demand ordinary-road route exploration through a routing API.
- **O5:** Socket.IO notifications and reconnection consistency.
- **O6:** Team-based engineering workflow with tests, traceability, and documentation.

### 1.4 Scope

**In scope for V1.0:** login, role permissions, scenario selection, interactive map, manually generated fictional incidents, basic fictional resource records, hydrant status and optional simulated inspection timestamp, incident histories, selected-route visualization, real-time status updates, documented error handling, automated tests.

**Potential V1.1 enhancements:** hydrant inspection history, scenario auto-generator, extended charts, multi-route comparisons, report export.

**Out of scope:** real alarms and sensor monitoring, actual fire response coordination, live emergency vehicle positions, verification of actual hydrants or water pressure, operational route recommendations, building hazard preplans, emergency medical records, real rescue and technical-response modules.

## 2. Stakeholders and User Classes

| Actor | Need | Authorization |
|---|---|---|
| **Viewer** | Explore simulations and view incident/resource information | Read-only |
| **Operator** | Create/update simulated incidents, request and inspect demo routes | Viewer rights + incident actions |
| **Administrator** | Manage users, scenarios and fictional resources | Operator rights + administration |
| **Routing Service (external)** | Return route geometry, distance and estimated ordinary-road duration | External API (not human user) |
| **Development Team / Evaluators** | Build, review and test the demo | According to assigned test roles |

Permission checks must run **server-side**; hiding buttons alone is insufficient.

## 3. Assumptions, Dependencies, and Constraints

- Two developers using VS Code, React/TypeScript, Express and PostgreSQL.
- Managed database hosting is proposed; no local heavy server requirement.
- Public basemap data and routing APIs may have quotas, usage restrictions, outages, licensing, and attribution requirements.
- Event and resource locations must be **fabricated for simulation**. Do not import real emergency service locations for the demo dataset.
- All scenario-bound reads and writes must enforce scenario isolation.
- The 10-week timeline and effort estimates are planning hypotheses, subject to review.

## 4. Functional Requirements

Priority scale: **Must** = MVP acceptance; **Should** = strong enhancement, implement only if time permits; **Could** = future option.

### 4.1 Authentication and Roles

| ID | Priority | Requirement |
|---|---|---|
| FR-01 | Must | Authorized users shall be able to sign in and sign out. |
| FR-02 | Must | The system shall support Admin, Operator and Viewer roles. |
| FR-03 | Must | The backend shall enforce role-based access on all protected endpoints. |
| FR-04 | Should | An Admin shall be able to create, deactivate, and update user accounts. |

### 4.2 GIS Dashboard

| ID | Priority | Requirement |
|---|---|---|
| FR-05 | Must | The frontend shall display a real geographic basemap with source attribution. |
| FR-06 | Must | Users shall be able to pan and zoom the map. |
| FR-07 | Must | Different markers shall identify **fictional** incidents, stations and hydrants. |
| FR-08 | Must | Clicking a marker shall display a simulated record summary. |
| FR-09 | Should | Users should be able to toggle selected map layers. |

### 4.3 Incident Lifecycle

| ID | Priority | Requirement |
|---|---|---|
| FR-10 | Must | An Operator or Admin shall create a simulated incident by selecting a permitted map location and entering required data. |
| FR-11 | Must | An incident shall have a unique ID, scenario, location, severity, status, source and creation timestamp. |
| FR-12 | Must | An authorized user shall update an incident using permitted state transitions. |
| FR-13 | Must | The system shall persist an audit-friendly history of state changes and who initiated them. |
| FR-14 | Should | The interface should filter incidents by status, severity or date. |
| FR-15 | Could | A scenario generator may create automatic **fictional** incidents. |

**Proposed state model:** `REPORTED → ACKNOWLEDGED → IN_PROGRESS → RESOLVED`; `CANCELLED` is a separate controlled end-state. Explicit transition rules are implemented and tested in the backend.

### 4.4 Fictional Resource Registry

| ID | Priority | Requirement |
|---|---|---|
| FR-16 | Must | An Admin shall manage fictional station records within a scenario. |
| FR-17 | Should | An Admin should manage fictional truck records linked to fictional stations. |
| FR-18 | Must | An Admin shall manage fictional hydrant records and their map coordinates. |
| FR-19 | Must | Users shall view only resources belonging to the selected scenario. |
| FR-20 | Must | Users shall explore relative proximity of fictional resources around a selected simulated event. |
| FR-21 | Should | Fictional hydrants should show simulated status and optional last-simulated-inspection date. |

"Nearby" is for exploratory visualization, not an assessment of real emergency suitability, water availability or dispatch readiness.

### 4.5 Route Exploration

| ID | Priority | Requirement |
|---|---|---|
| FR-22 | Must | The backend shall integrate an external ordinary-road routing service through an adapter. |
| FR-23 | Must | An Operator shall request a route between a selected **fictional station** and **simulated incident**. |
| FR-24 | Must | The UI shall show API-returned distance and estimated travel duration with an explanatory simulation label. |
| FR-25 | Must | The frontend shall draw the API-returned route geometry if available. |
| FR-26 | Should | The system should compare multiple user-selected candidate routes using a stated metric. |
| FR-27 | Must | Routing failures, quota exhaustion or unsupported paths shall show an error/empty state; the system shall not invent a successful route. |

Do **not** describe these outputs as verified emergency-optimal routes or use them to direct real response teams.

### 4.6 Real-Time Updates, Reporting and Scenarios

| ID | Priority | Requirement |
|---|---|---|
| FR-28 | Must | New simulated incidents shall notify connected viewers of the same scenario. |
| FR-29 | Must | Incident state changes shall synchronize across connected clients. |
| FR-30 | Should | Changes to fictional resources should also synchronize. |
| FR-31 | Must | On reconnection the client shall re-fetch authoritative state. |
| FR-32 | Should | The dashboard should count simulated incidents by status. |
| FR-33 | Must | Users shall be able to view incident history. |
| FR-34 | Should | The dashboard should show basic historical charts. |
| FR-35 | Could | The system may export a clearly labeled simulation report. |
| FR-36 | Must | Authorized users shall be able to select a predefined simulation scenario. |
| FR-37 | Must | Scenario-bound records and realtime messages shall be isolated by scenario. |
| FR-38 | Should | Admins should be able to reset a scenario to its fictional seed data. |

### 4.7 Fire Hydrant Inspection Enhancement (V1.1)

| ID | Priority | Requirement |
|---|---|---|
| FR-39 | Could | Admins may create an explicitly **simulated** hydrant inspection history record. |
| FR-40 | Could | Authorized users may browse inspection history and maintenance notes for fictional hydrants. |

## 5. Non-Functional Requirements

Targets below are **proposed test targets**, not measured guarantees. Test conditions and exceptions must be documented.

| ID | Attribute | Requirement / Proposed verification |
|---|---|---|
| NFR-01 | Performance | In a defined local/demo environment, at least 95% of ordinary database-backed API calls complete within 500 ms; exclude third-party routing time. |
| NFR-02 | Responsiveness | In a stable development network, 95% of real-time change notifications arrive within 2 s. |
| NFR-03 | Usability | During a short usability test, a new user can create a simulated incident and locate its marker after a brief introduction. |
| NFR-04 | Compatibility | Support current desktop Chrome, Edge and Firefox; basic small-screen layout remains functional. |
| NFR-05 | Security | Password hashes, authenticated sessions, role checks and input validation; secrets never exposed in frontend or repository. |
| NFR-06 | Reliability | Successfully committed incidents and history survive application restart. |
| NFR-07 | Resilience | Routing outages produce clear errors; Socket.IO reconnection re-synchronizes state. |
| NFR-08 | Maintainability | Frontend components and backend modules are separated by responsibility; peer-reviewed changes. |
| NFR-09 | Testability | Incident state rules, coordinates, authorization and routing adapter have automated tests. |
| NFR-10 | Data integrity | Enforce foreign keys, mandatory fields, allowed statuses and scenario ownership. |
| NFR-11 | Safety / clarity | All incident and emergency-resource views display conspicuous 'SIMULATION ONLY — NOT FOR EMERGENCY USE' context. |
| NFR-12 | Legal/API terms | Use maps with required attribution; obey API licensing and usage limits. |
| NFR-13 | Data minimization | Do not store real emergency reports, medical details, or unnecessary personal data. |
| NFR-14 | Recovery | Document steps to recreate a development database from migrations and fictional seed data. |

See [Quality Assurance Plan](../quality/quality-assurance-plan.md) for test cases and release gates.

## 6. Core Use Cases

### UC-01 — Create Simulated Incident

- **Actor:** Operator or Admin
- **Preconditions:** Logged in, scenario selected, write permission granted
- **Flow:** open map → select 'Create Simulated Incident' → select location → enter required details → submit → backend validates and persists → marker and notification appear.
- **Failure:** invalid coordinate, missing data, unauthorized role, failed database write → descriptive error, no partial record.
- **Postcondition:** incident exists in selected scenario, with history created as applicable.

### UC-02 — View Fictional Resource Information

- **Actor:** Viewer, Operator, Admin
- **Flow:** select scenario → open map → enable markers → click resource marker → view simulated identity, status and inspection timestamp when available.
- **Postcondition:** no data mutation.

### UC-03 — Explore Demonstration Road Route

- **Actor:** Operator or Admin
- **Flow:** choose incident + fictional station → request route → backend validates scenario ownership → routing adapter calls API → map renders returned line and summary.
- **Failure:** API error/empty route → show 'Route unavailable'; no invented values.
- **Postcondition:** route is visualized as **ordinary-road demonstration**, not a dispatch recommendation.

### UC-04 — Update Incident State

- **Actor:** Operator or Admin
- **Flow:** open incident → select allowed next state → backend enforces permission and transition → write change + history in one transaction → broadcast update.
- **Failure:** stale edit, invalid transition, database error → reject and re-fetch authoritative state.

### UC-05 — Observe Real-Time Change

- **Actor:** Viewer, Operator, Admin
- **Flow:** join selected scenario → receive event → update UI or fetch changed record → reconnect when needed → re-fetch state to avoid missed messages.

### UC-06 — Manage Fictional Resources

- **Actor:** Admin
- **Flow:** select scenario → add/edit/deactivate a fictional station/hydrant (and trucks if enabled) → validate data → save → reflect on map.

## 7. Data Requirements / Proposed Entities

| Entity | Key attributes | Key relationships |
|---|---|---|
| `users` | id, name, email, password_hash, role, created_at | user referenced from incident history |
| `scenarios` | id, name, description, status, created_at | parent of resources and incidents |
| `fire_stations` | id, scenario_id, name, latitude, longitude, status | station belongs to scenario |
| `fire_trucks` | id, station_id, vehicle_code, status | truck belongs to a fictional station |
| `fire_hydrants` | id, scenario_id, code, latitude, longitude, status, `last_simulated_inspection_at` (nullable) | hydrant belongs to scenario |
| `fire_incidents` | id, scenario_id, code, latitude, longitude, severity, status, reported_at | incident belongs to scenario |
| `incident_history` | id, incident_id, user_id, old_status, new_status, created_at | event and acting user |
| `route_results` | id, incident_id, station_id, source, distance_m, duration_s, route_geometry, calculated_at | route linked to event and station |
| `incident_assignments` | id, incident_id, truck_id, assigned_at, status | demonstration linkage only; no real dispatch |

Optional V1.1 table: `hydrant_inspections(inspection_id, hydrant_id, inspector_id, inspection_date, status, remarks)`.

**Integrity note:** The ERD is a design draft. During migration design, explicitly enforce that linked stations, trucks and incidents are within the same scenario. `route_geometry` can be GeoJSON `JSONB` at MVP stage; PostGIS `geometry/geography` and indexing are optional optimizations. PostgreSQL and the mapping UI must use a documented coordinate convention: Leaflet often accepts `[lat, lon]`; many route APIs accept `[lon, lat]`.

## 8. Interfaces and API Contracts (Draft)

| Method | Endpoint | Purpose | Role |
|---|---|---|---|
| POST | `/api/v1/auth/login` | Login | Public |
| POST | `/api/v1/auth/logout` | Logout | Authenticated |
| GET | `/api/v1/scenarios` | List visible scenarios | Authenticated |
| GET | `/api/v1/scenarios/:id/incidents` | Scenario incident list | Viewer+ |
| POST | `/api/v1/incidents` | Create simulated incident | Operator+ |
| GET | `/api/v1/incidents/:id` | Incident detail | Viewer+ |
| PATCH | `/api/v1/incidents/:id/status` | Controlled state transition | Operator+ |
| GET | `/api/v1/incidents/:id/history` | Event history | Viewer+ |
| GET | `/api/v1/scenarios/:id/stations` | Fictional stations | Viewer+ |
| GET | `/api/v1/scenarios/:id/hydrants` | Fictional hydrants | Viewer+ |
| POST | `/api/v1/incidents/:id/routes` | Request demonstration route | Operator+ |

**WebSocket events:** `incident:created`, `incident:updated`; optional `resource:updated`; on reconnect, refresh current scenario data. Event rooms must not disclose data across scenarios.

## 9. Acceptance Criteria (MVP)

| ID | Acceptance criterion | Evidence |
|---|---|---|
| AC-01 | Authorized user can sign in and out. | E2E login/logout |
| AC-02 | Map renders with attribution and basic interaction. | Browser test + screenshot |
| AC-03 | Selected scenario displays distinct fictional resource markers. | E2E / component test |
| AC-04 | Operator can create a simulated incident from map position. | E2E create test |
| AC-05 | Incident remains after page refresh and server restart. | Persistence integration test |
| AC-06 | Viewer can explore fictional resource summaries. | Browser acceptance test |
| AC-07 | At least one supported demo station-incident pair renders an actual routing-service response. | Integration test with approved demo coordinates |
| AC-08 | Two connected browsers observe an incident status change without manual refresh. | Two-client test |
| AC-09 | Viewer direct API writes are rejected with 403. | API authorization test |
| AC-10 | No fabricated route appears when routing API fails. | Mock outage test |
| AC-11 | Simulated incident/resource pages visibly state 'SIMULATION ONLY'. | UI checklist / E2E |
| AC-12 | CI runs relevant tests; setup and seed scripts are documented. | GitHub Actions evidence |
| AC-13 | Cross-scenario data access and assignments are rejected. | Database/API integration tests |

## 10. Requirements Traceability (Initial)

| Objective | Requirements | Main verification |
|---|---|---|
| O1 | FR-05–09 | AC-02–03 |
| O2 | FR-10–14, FR-33 | AC-04–05 |
| O3 | FR-16–21, FR-36–38 | AC-03, AC-06, AC-13 |
| O4 | FR-22–27 | AC-07, AC-10 |
| O5 | FR-28–31 | AC-08 |
| O6 | FR-01–04 and NFRs | AC-01, AC-09, AC-11–13 |

## 11. Open Decisions (Team Sign-Off Required)

1. Whether user registration is admin-only or seeded accounts for classroom demo.
2. Which routing provider/plan is suitable under current quota, cost and terms.
3. Whether PostGIS is needed from the first migration or can be deferred.
4. Which fictional scenario and approximate demo footprint to seed.
5. Whether fictional trucks are fully managed in V1.0 or only represented as sample records.
6. Which numeric NFR test conditions and devices will be used for final acceptance.

## 12. Change Control

A new/changed requirement needs: a GitHub Issue with rationale, Must/Should/Could priority, effort/risk impact, acceptance criteria, and approval from both collaborators. Update SRS, diagrams, tests and milestones in the same PR where practical. Freeze MVP scope once Phase 2 begins; changes to Must items require explicit agreement.

## 13. Related Documents

- [Project README](../../README.md)
- [Cost Estimation](../project-management/cost-estimation.md)
- [Risk Register](../project-management/risk-register.md)
- [Communication Plan](../project-management/communication-plan.md)
- [Quality Assurance Plan](../quality/quality-assurance-plan.md)

---

**Approval:** Pending review by both developers.  
**Version:** Draft 0.2; do not treat this as finalized implementation behavior.
