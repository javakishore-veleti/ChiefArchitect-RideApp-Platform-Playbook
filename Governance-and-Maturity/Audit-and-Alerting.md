# Audit Logging and Alerting Policies – RideShareApp Platform

## Objective
Establish a consistent, enforceable policy framework for audit logging and alerting across the RideShareApp Platform, ensuring visibility into sensitive actions, compliance readiness, and real-time threat detection.

---

## 1. What Should Be Audited
| Domain               | Audit Events                                                           |
|----------------------|-------------------------------------------------------------------------|
| Rider App            | Login attempts, location tracking opt-in/out, payment method updates    |
| Driver App           | Shift start/end, license verification, bank detail edits                |
| Admin Portals        | Fare rule changes, SLO overrides, manual driver adjustments             |
| ML Systems           | Model promotion, inference config changes, dataset access requests      |
| APIs & Backend       | Token issuance/revocation, API key usage, PII field access              |

---

## 2. Logging Requirements
- **Standard Format**: JSON logs with fields: `actor_id`, `action`, `timestamp`, `geo`, `scope`, `status`, `trace_id`
- **Immutability**: All audit logs stored in write-once BigQuery tables with 365-day retention
- **PII Access Events**: Must be tagged with domain-specific labels (e.g. `pricing`, `driver_verification`)
- **Correlation ID Propagation**: All services must include `x-trace-id` for multi-service visibility
- **Restricted Console Logs**: Secrets, tokens, or PII must never be printed in stdout/stderr

---

## 3. Alerting Policies
| Severity | Triggers                                                                 | Action                                                                          |
|----------|--------------------------------------------------------------------------|---------------------------------------------------------------------------------|
| Critical | Unauthorized schema change, root role use outside change window          | PagerDuty alert, freeze affected IAM roles                                     |
| High     | Model update without signed commit, admin API misuse                    | Slack channel alert, audit review triggered                                    |
| Medium   | Suspicious location spoofing attempts or rapid API key rotation         | Daily alert digest to security leads                                           |
| Low      | Missing audit logs in CI/CD job context, config drift in logging setup  | Weekly review by Platform Infra                                                |

---

## 4. Tooling
- **Centralized Log Ingestion**: Logs flow through FluentBit → Pub/Sub → Dataflow → BigQuery → AlertManager
- **Real-Time Rules**: Cloud SIEM integration (e.g. Chronicle, Panther) with custom detections per microservice
- **Audit Log Validator**: GitHub Action to detect missing audit hooks in PRs (e.g. no `record_audit_log()` call)

---

## 5. Governance and Compliance
- **Quarterly Reviews**: Audit trail completeness and fidelity scored by Platform Security
- **Privacy Audit Hooks**: Validated by Privacy Review Board for all features triggering PIAs
- **Retention Policies**: Configurable per region (e.g., 730 days in EU, 180 days in India) as per DPDP/GDPR

---

## 6. Developer Guidance
- **Audit Hook Snippets**: Available in shared libs (`platform-audit-lib`)
- **Onboarding Lint Checks**: CI ensures all new services implement logging interceptor
- **Audit Dashboard**: Backstage plugin shows gaps, freshness, and action items by team

---

## Summary
Audit logging and alerting form the backbone of trust, security, and operational transparency in the RideShareApp Platform. From regional compliance mandates to real-time anomaly detection, this policy ensures that every sensitive action is recorded, traceable, and auditable.
