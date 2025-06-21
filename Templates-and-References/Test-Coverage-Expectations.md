# Test Coverage Expectations – RideShareApp Platform

## Objective
Establish platform-wide expectations and quality bars for automated test coverage across all services, libraries, APIs, and ML pipelines within the RideShareApp Platform.

---

## 1. Coverage Targets by Component

| Component                            | Unit Tests | Integration Tests | E2E Tests | Notes |
|--------------------------------------|------------|-------------------|-----------|-------|
| Microservices (e.g. `ride-matcher`)  | >= 80%     | >= 60%            | Selective | Covers core ride lifecycle APIs (e.g., booking, matching, cancellation) |
| Shared Libraries (`rideapp-utils`)   | >= 90%     | -                 | -         | Utilities used across ETA prediction and pricing engines |
| ML Pipelines (e.g. `eta-predictor`)  | >= 70%     | >= 60%            | Batch test | Model versioning, training/serving validation required |
| API Gateway/Router                   | >= 85%     | >= 70%            | Contract tests | Covers route resolution, request transformation, and throttling |
| Platform CLI & Internal Tools        | >= 75%     | -                 | Manual ok | Examples: `rideapp-deploy-cli`, `dataset-uploader`, `model-audit` |

---

## 2. Required Test Types
- Unit Tests: Core domain logic (e.g., fare calculation, surge band assignment)
- Integration Tests: PostgreSQL, Redis, Kafka, and external APIs (e.g., maps, payments)
- Contract Tests: OpenAPI/gRPC interfaces tested via tools like Dredd or Pact
- E2E Tests: Booking flow from `create_ride → assign_driver → trip_end` on staging
- Regression Suites: Scheduled runs on representative synthetic datasets

---

## 3. PR Checklist
All pull requests must include:
- Added or updated unit/integration tests
- Local test suite and CI workflows passing
- Coverage report visible to reviewers
- Linked to JIRA or GitHub Epic for traceability

---

## 4. Tooling
- Language runners: JUnit (Java), pytest (Python), Go test, Jest (Node.js)
- CI Integration: GitHub Actions using `rideapp-test-runner` and `schema-checker`
- Coverage: Codecov or native tools (e.g., Jacoco, Coverage.py)
- Static Analysis: SonarQube rules tailored for streaming logic, API boundaries, and schema safety

---

## 5. Quality Gates
- PR blocked if coverage drops by more than 2% without explanation
- New microservices (e.g., surge-pricing-engine) must meet thresholds before prod rollout
- Justification required in PR description for any known test gaps over 10%
- Flaky test tracking integrated with developer scorecards

---

## 6. Reporting & Governance
- Weekly reports by platform domain (e.g., ETA, DriverOps, SurgePricing)
- ML pipelines tracked via `model-registry-test-metrics` dashboard
- Flaky test review in team retros every 2 weeks
- Tagging of tests with labels like `critical_path`, `realtime`, `batch`, `tenant_sensitive`

---

## Summary
Enforcing clear and measurable test coverage expectations ensures that the RideShareApp Platform maintains high-quality services across both real-time operations and ML-powered intelligence. These expectations reinforce engineering accountability, support faster recovery from regressions, and enable scalable delivery practices across the entire engineering organization.
