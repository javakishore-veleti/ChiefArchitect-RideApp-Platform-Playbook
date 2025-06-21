# Data Retention Policy – RideShareApp Platform

## Objective
Establish data retention standards across the RideShareApp Platform to ensure compliance with legal requirements, optimize storage costs, and minimize risk exposure while preserving analytical and operational value.

---

## 1. Retention Principles
- **Regulatory Compliance:** Meet obligations under GDPR, CCPA, local mobility and taxation laws.
- **Minimization:** Retain only the data needed for business operations, auditability, or model retraining.
- **Access Boundaries:** Align retention with RBAC and classification policies.
- **Data Lifecycle Visibility:** Every data asset must have a defined TTL (Time to Live) policy.

---

## 2. Retention Periods by Data Type

| Data Category                  | Retention Period | Storage Tier          | Deletion Policy                     |
|-------------------------------|------------------|------------------------|--------------------------------------|
| Trip Events (rider, driver)   | 18 months        | Hot (Data Lake + BQ)   | Auto-expire via lifecycle policy     |
| Payment and Invoice Data      | 7 years          | Warm (SQL + Archive)   | Manual archive with audit trail      |
| Driver Compliance Documents   | 3 years          | Secure Encrypted Store | Rotated quarterly, policy-reviewed   |
| Location History              | 30 days          | Stream Storage (Kafka) | Auto-delete via TTL config           |
| Model Training Features       | 12 months        | Cold (S3 Parquet)      | Replace on retrain or expiry         |
| Anonymized Analytics Data     | Indefinite*      | Aggregated BQ Tables   | Purgeable if requested               |

> * Indefinite retention allowed only if data is fully anonymized and used for product insights.

---

## 3. Tooling & Automation
- Lifecycle policies managed via:
  - BigQuery TTL partitions
  - S3 object lifecycle rules
  - Kafka topic retention configs
  - Vault TTLs for secrets

- Audit logs tracked for:
  - Manual deletions
  - Access beyond retention window
  - TTL policy exceptions

---

## 4. Enforcement
- CI check on schema change PRs requires `retention_period` metadata tag
- Dashboard for expiring datasets and overdue purges
- Monthly job for purging expired feature store and location logs
- Data Engineering owns TTL policy sync across storage systems

---

## 5. Legal & Security Alignment
- Reviewed quarterly with Security, Legal, and Data Governance
- Special handling of rider PII: masked beyond 90 days in non-essential use cases
- DSR (Data Subject Request) support workflows built into storage APIs

---

## Summary
This data retention policy provides the RideShareApp Platform with guardrails to manage data ethically, legally, and cost-effectively. It ensures clarity across systems, aligns with international compliance needs, and promotes operational cleanliness through automation and visibility.
